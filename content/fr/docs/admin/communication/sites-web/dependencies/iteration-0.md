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

## Cas d'usage

### Pages

1. Je crée une page, je la publie, il faut qu'elle soit exportée
2. J'ajoute une image à une page, il faut que l'image soit exportée
3. Je change l'image, il faut supprimer la précédente et exporter la nouvelle
4. Je crée une page enfant, il faut qu'elle soit exportée
5. Je change le chemin de la page parent, il faut que le parent et l'enfant soient exportés
6. J'ajoute un bloc "Personnes", je lie une personne, il faut que la page soit exportée, ainsi que la personne et sa photo
7. Je change le chemin d'une page utilisée dans un élément de menu, il faut exporter la page et le menu

### Actualités

Les scenarii 1 à 3 ne sont pas repris, bien qu'ils soient pertinents pour les actualités et toutes les dépendances natives des sites.

1. Je crée une actualité, je la publie avec une date dans le futur. Il faut qu'elle ne soit pas publiée, jusqu'à la dite date
2. Je crée une actualité, il faut exporter toutes les pages dotées d'un bloc "actualités" afin de mettre à jour les listes

Ce cas "2." peut être traité de 2 façons :
- en listant explicitement les articles concernés (c'est la situation actuelle), ce qui donne au CMS la charge des calculs et qui permet à Hugo de simplement récupérer ce qu'on lui demande
- en indiquant les règles à Hugo (la catégorie et le nombre d'articles), ce qui donne à Hugo la charge des calculs et permet de ne pas mettre à jour les pages présentant ces blocs.

### Catégories

1. Je renomme une catégorie. Il faut l'exporter, exporter sa catégorie parente (qui liste les enfants), exporter ses catégories enfants, et exporter tous les articles liés à la catégorie et à sa descendance pour faire correspondre le chemin.
2. Je déplace une catégorie. Il faut l'exporter, exporter l'ancien parent, le nouveau parent, toute la descendance, et exporter tous les articles liés à la catégorie et à la descendance.
3. Je change le chemin d'une catégorie utilisée dans un élément de menu, il faut exporter la catégorie et le menu.

### Personnes

1. J'ajoute un bloc "chapitre" à une personne, avec une image. Il faut que cette image soit exportée pour tous les sites liés
2. J'ajoute un bloc "organisations" à une personne, avec une organisation liée. Il faut que l'organisation et son logo soient exportés pour tous les sites liés

### Formations

1. Je définis un site comme site d'école. Il faut que les formations de l'école soient exportés dans le site
2. J'ajoute un bloc "formations" dans un site. Il faut restreindre les formations proposées aux formations liées au site
3. J'ajoute un enseignant à une formation. Il faut que l'enseignant et la personne soient exportés dans tous les sites liés
4. J'ajoute un bloc "organisations" pour lister des partenaires. Il faut que les partenaires et leurs logos soient exportés vers tous les sites liés.

Le cas "2." est important, et n'est pas implémenté tel quel aujourd'hui.
Dans le site de l'IUT Bordeaux Montaigne, un bloc "Formations" ne doit permettre de lister que des formations de l'IUT.
Sinon, l'ajout d'une formation que l'école n'assure pas ajouterait cette formation à l'offre de formation présentée sur le site.


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
