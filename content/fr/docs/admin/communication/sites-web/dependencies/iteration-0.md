---
title: Itération 0
description: Début de la réflexion
---

## Connexions

### Objets du site (connexions directes)
Les objets suivants ont tous une propriété `website` qui les relie directement au site, dans une relation d'appartenance `belongs_to` :
- pages
- actualités
- catégories
- menus

Chacun de ces objets est enregistré en base de données avec un lien au website, ces objets sont donc simples à gérer.
Il suffit de déclencher un export à chaque événement (création, modification, suppression).

### Images à la une
Il faut lister les images ("image à la une") de tous les objets connectés pour les connecter au site Web, et tenir à jour cette liste.
Les images en question s'appuient sur [Active Storage](https://guides.rubyonrails.org/active_storage_overview.html), qui utilise des objets `Blob` en base de données.
Ces objets sont créés et détruits mais jamais modifiés, il faut donc suivre uniquement les créations et suppressions.

### Blocs de base, narratifs et techniques
Les objets qui utilisent des blocs peuvent présenter des dépendances.
Ces blocs peuvent utiliser des images ou des fichiers, qu'il faut lister et suivre pour les ajouter ou les supprimer de la liste des médias.

### Blocs de liste
Ces blocs créent des liens entre objets.
Exemple : une page utilise un bloc "Organisations", qui fait référence à 5 organisations.
Les 5 organisations sont des dépendances de la page, parce qu'elles sont nécessaires pour le bon affichage.
Les objets ont eux-mêmes des images (le logo de l'organisation, la photo d'une personne), et surtout ils ont des blocs.

### Autres dépendances indirectes
Les formations et les écoles ont du personnel enseignant et administratif qui sont donc des dépendances indirectes.
De même, les revues scientifiques ont des auteurs, et les auteurs ont des publications (pas des papiers, c'est subtil...).

### Le problème de la boucle infinie
On peut donc arriver à une référence circulaire : une personne mentionne une organisation, cette organisation mentionne la personne.
Si l'on code un algorithme récursif qui suit la chaîne de dépendances, cette boucle est un piège logique, la mise à jour ne finira jamais, on tombe dans une boucle infinie.

### L'indirect non listé
Il peut arriver qu'une personne ou une organisation doive être ajoutée à un site Web, alors qu'elle n'apparaît dans aucun bloc ni aucune formation.
Il faut dans ce cas pouvoir l'ajouter explicitement, en utilisant une interface sur la page des personnes.


## Réflexions sur les dépendances de référence et les événements déclencheurs

L'ensemble de ces objets doivent être mis à jour dans certaines conditions.

Certains événements sur un objet impliquent des mises à jour des objets dépendants.
Ces mises à jour concernent les objets listés avant l'événement (par exemple les anciennes catégories, l'ancien parent), et les objets après l'événement (les nouvelles catégories, le nouveau parent).

Si le chemin (`path`) change, ou si une relation change, l'idée est qu'on doit mettre à jour d'autres objets, parce qu'ils utilisent le `path` de l'objet pour y faire référence.
Par exemple :
- une page change de slug -> le path change + la descendance
- une page change de parent -> le path change + l'ancien parent qui listait ses enfants change
- une page change de catégorie -> les objets qui référencent l'ancienne et la nouvelle catégorie changent (les blocs actu désignant ces catégories)

Ces relations ne sont pas des dépendances d'affichage, parce que les objets ne sont pas indispensables à l'affichage.
En revanche, ils partagent des caractéristiques :
- quand on change un `path`, il faut mettre à jour toute la descendance
- quand on déplace un objet, il faut mettre à jour le précédent parent, le nouveau parent, et toute la descendance
- quand on ajoute ou supprime une relation, il faut regénérer les dépendances de structure

Certains de ces objets présentent des liens entre eux.
1. Les pages, actualités et catégories peuvent être utilisés comme éléments de menu.
Un changement de `path` implique donc de mettre à jour le menu qui s'y réfère.
2. Les catégories sont utilisées pour organiser les actualités.
Une mise à jour du `path` de catégorie implique de modifier les actualités liées.
3. Les pages et les catégories sont arborescentes, donc il faut mettre à jour toute la descendance quand le `path` change.
4. Les `path` des actualités dépendent du `path` de la page spéciale "Actualités", qui sert d'index.
Si cette page est renommée "News", il faut mettre à jour toutes les actualités pour que "actualites/..." devienne "news/...".

Ces cas nécessitent, à l'enregistrement d'un objet et si son `path` a évolué, de mettre à jour d'autres objets en cascade.
