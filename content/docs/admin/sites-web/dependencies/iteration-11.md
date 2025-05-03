---
title: Itération 11
description: Réflexion 28 avril 2025
weight: 11
---

Il est peut-être pertinent de couper le processus actuel en 3 morceaux :
1. l'identification des fichiers possiblement modifiés
2. la génération de ces fichiers, avec stockage dans les `git_files`
3. la synchronisation sur Git des fichiers modifiés

## Phase 1 — Identification

L'idée est, à la sauvegarde d'un objet, de déclencher un certain nombre de jobs visant à regénérer chaque dépendance et chaque référence.
Le fait de créer une tâche pour chaque permet de bénéficier du dédoublonnage de GoodJob.

## Phase 2 — Génération

La génération consiste, pour un `git_file`, à :
1. faire le rendu du fichier statique
2. le comparer au contenu stocké
3. s'il est identique, ne rien faire
4. s'il est différent, faire les tâches ci-après

Tâches à faire :
- calculer le hash
- enregistrer le contenu dans le `git_file`,
- noter que l'objet nécessite une synchronisation
- noter la date de désynchronisation

> [!TIP] Question
> En cas de multiples mises à jour, faut-il garder la première date de désynchro ou la dernière ? En l'état actuel, on garde la dernière.

Ces tâches vont permettre de supprimer les tentatives de cache de certains fichiers statiques, qui ne sont plus nécessaires.

## Phase 3 — Synchronisation

Pour chaque site, il devient donc possible de savoir le nombre de fichiers nécessitant une synchronisation.
On peut alors, sans se préoccuper de la cause ou de la répétition, générer des lots de synchronisation.
On pourrait imaginer un automate qui passe toutes les 10 minutes et crée des lots de 100 objets.
On pourrait imaginer aussi un bouton au niveau du site pour déclencher manuellement la mise à jour.

La mise à jour se fait comme suit :
1. choix des fichiers à envoyer (par date, avec des limites de quantité)
2. création d'un tree avec les nouvelles versions des git files
3. création d'un commit lié au tree
4. push
5. mise à jour des hash et des infos de synchronisation des fichiers concernés, notamment la suppression de la date de désynchro.

## Nettoyage nocturne

Dans le `clean_and_rebuild`, on peut imaginer une première tâche qui met à jour tous les hash réels des `git_files` du site côté Osuny, afin d'utiliser une seule requête d'API Gihub/Gitlab.
Cette tâche doit s'effectuer au début, de façon à créer un arbre dans Osuny fidèle à l'arbre Github/Gitlab.
Cette mise à jour des hash git génère des désynchronisations, qui doivent ensuite être traitées dans le flux normal.

## Bénéfices

### Simplifier les algos

Au lieu d'un algo qui s'occupe des 3 sujets, on a 3 algos plus petits.

### Séparer les opérations des commits

Les opérations dans Osuny (créations, enregistrements, suppressions) ne donnent pas immédiatement suite à une synchronisation complète.
Cela évite les salves de commits alors qu'on travaille souvent sur les différents blocs d'un même objet.

### Gérer les files d'attente nativement

5 modifications du même bloc ne généreront pas 5 tâches de synchronisation, elles seront dédoublonnées.

### Accélérer les traitements

Chaque opération est plus simple, on n'a plus d'éléphants ou de baleines qui mélangent analyse, génération et synchronisation.

### Diminuer la RAM

Le fait de séparer la génération de l'envoi limite l'usage de la RAM, parce qu'on a besoin de monter les objets en mémoire uniquement pendant l'identification et la génération.
- L'identification reste à coût identique en RAM.
- La génération va baisser parce qu'elle est faite objet par objet.
- La synchronisation va baisser radicalement parce que le contenu statique est déjà généré en base de données.

### Permettre la parallélisation

La génération ne pose plus de problème de race condition.

## Risques

### Augmenter le nombre de tâches

Le fait de fragmenter va générer beaucoup plus de tâches qu'aujourd'hui

### Alourdir la base de données

Stocker les contenus statiques dans la base va alourdir.
Ce n'est que du texte, mais il y en a beaucoup.
Si c'est trop, on peut sortir les fichiers, soit dans une autre base soit sur un Object Storage.

## Migration

### Point unique de migration

Le site web devient l'unique point d'entrée permettant de synchroniser.
Ainsi, le code 
```ruby
  def publish
    @l10n.publish!
    @event.sync_with_git
    ...
  end
```
devient 
```ruby
  def publish
    @l10n.publish!
    @website.sync_with_git
    ...
  end
```

### Nouvelles méthodes

La méthode asynchrone connect to websites permet de connecter les objets indirects aux sites.
Cela se passe en 2 temps : d'abord se connecter aux objets directs, puis générer les git_files nécessaires, dans chaque site.

```ruby
  def connect_to_websites
    Communication::Website::IndirectObject::ConnectToWebsitesJob.perform_later self
  end

  def connect_to_websites_safely
    transaction do
      connect_direct_sources
      generate_git_files
    end
  end
```

### Méthodes obsolètes

Toutes les méthodes qui déclenchent des synchronisations aux opérations de CRUD :
- `save_and_sync` 
- `update_and_sync`
- `sync_with_git`
Cela redevient des `save`, `update` et `touch` normaux, et c'est le `save` qui initie la connexion aux sites.


Toutes les méthodes qui visaient à minimiser le nombre de synchronisations disparaissent : 
- `update_columns` dans les reorder
- `reorder_object`
- `sync_after_reorder`
- `trigger_category_sync`

Toutes les méthodes qui mettent en pause les callbacks :
- `pause_git_sync`
- `unpause_git_sync`

### Nouveaux jobs

Un nouveau job apparaît, permettant de réaliser la phase 2, de génération :
- `Communication::Website::GitFile::GenerateContentJob`

### Jobs obsolètes

Les jobs de synchonisation disparaissent : 
- `Communication::Website::DirectObject::SyncWithGitJob`
- `Communication::Website::IndirectObject::ConnectAndSyncDirectSourcesJob`

### Nettoyage

Le concern `WithGitFiles` est renommé `HasGitFiles` afin de respecter l'usage de `With` pour les traits uniquement.

Les méthodes `sync_with_git` et `sync_with_git_safely` du site web se déplacent dans le trait `WithGitRepository`.

La méthode `syncable?` est problématique, parce qu'elle indique si le contenu est publié, et qu'elle est définie par le concern `WithDependencies`. `WithDependencies` est chargé par les blocs, les templates, les sites, les objets directs et indirects. Elle a été pensée pour avoir des overrides, mais en fait elle n'en a pas. La seule autre définition est dans `ActiveStorage::Blob`. Cela fait doublon avec `exportable_to_git?`, qui répond à la même question mais sans regarder la publication. Comment clarifier ?

### Synchronisation automatique

Une routine s'exécute à intervalles réguliers (toutes les 10 minutes pour commencer) afin de synchoniser les sites qui en ont besoin.

### Synchronisation manuelle

Un nouveau bouton permet d'indiquer le nombre de fichiers désynchronisés, et de lancer une migration sans attendre la routine régulière. Le site est doté d'une propriété `last_sync_at` afin de permettre de ne pas compter les fichiers en cours de synchronisation (dans une tâche en arrière-plan).

### Monitoring des git_files

De nouvelles routes permettent d'examiner les git_files des sites web. 
Ces routes ne sont pas interfacées.
````
/admin/fr/communication/websites/:website_id/git_files
/admin/fr/communication/websites/:website_id/git_files/:id
```