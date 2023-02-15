---
title: Connexions et dépendances
weight: 3
description: Envoyer tous les fichiers nécessaires
---

Pour être mis en ligne, les sites doivent être envoyés sur un référentiel Git. 
Pour cela, il faut lister tous les objets nécessaires et les mettre à jour s'ils ont évolué.
Certains doivent être créés, d'autres modifiés, d'autres supprimés.
Le processus est documenté dans la page [Export Hugo](/docs/admin/communication/sites-web/export/).

La question traitée ici est celle de la liste des objets, et de la mise à jour efficiente :
- comment ajouter les objets à la liste quand c'est nécessaire ?
- comment déclencher des mises à jour les plus précises possibles (ni trop, ce qui causerait du calcul inutile, ni trop peu, ce qui causerait des manques de données) ?
- comment supprimer les objets quand c'est nécessaire ?

## Dépendances directes

### Natives
Un site Web présente des dépendances directes "natives", ou "structurelles" :
- les pages
- les actualités
- les catégories
- les menus
- les médias
- des fichiers de configurations divers

Chacun de ces objets est enregistré en base de données avec un lien au website, ces objets sont donc simples à gérer. 
Il suffit de déclencher un export à chaque événement (création, modification, suppression).

### Images
Il faut lister les images ("image à la une") utilisées par ces objets pour les ajouter à la liste des médias, et mettre à jour cette liste.
Les images en question s'appuient sur [Active Storage](https://guides.rubyonrails.org/active_storage_overview.html), qui utilisent des objets `Blob` en base de données.
Ces objets sont créés et détruits mais jamais modifiés, il faut donc suivre uniquement les créations et suppressions.

### Références
Certaines dépendances natives présentent des liens entre elles.
- Les pages, actualités et catégories peuvent être utilisés comme éléments de menu.
Un changement de `path` implique donc de mettre à jour le menu qui s'y réfère.
- Les catégories sont utilisées pour organiser les actualités.
Une mise à jour du `path` de catégorie implique de modifier les actualités liées.
- Les pages sont arborescentes, donc il faut mettre à jour toute la descendance quand le path change.

Ces 2 cas nécessitent, à l'enregistrement d'un objet et si son `path` a évolué, de mettre à jour d'autres objets en cascade.

## Dépendances indirectes

Les objets qui utilisent des blocs peuvent présenter des dépendances indirectes.

### Blocs de base, narratifs et techniques
Ces blocs peuvent utiliser des images, qu'il faut lister et suivre pour les ajouter ou les supprimer de la liste des médias. 

### Blocs de liste
Ces blocs créent des liens entre objets. 
Exemple : une page utilise un bloc "Organisations", qui fait référence à 5 organisations. 
Les 5 organisations sont des dépendances indirectes de la page.
Les objets ont eux-mêmes des images (le logo de l'organisation, la photo d'une personne), et surtout ils ont des blocs.

### Le problème de la boucle infinie
On peut donc arriver à une référence circulaire : une personne mentionne une organisation, cette organisation mentionne la personne.
Si l'on code un algorithme récursif qui suit la chaîne de dépendances, cette boucle est un piège logique, la mise à jour ne finira jamais, on tombe dans une boucle infinie.

### Autres dépendances indirectes
Les formations et les écoles ont du personnel enseignant et administratif qui sont donc des dépendances indirectes.
De même, les revues scientifiques ont des auteurs, et les auteurs ont des publications (pas des papiers, c'est subtil...).

### L'indirect non listé
Il peut arriver qu'une personne ou une organisation doive être ajoutée à un site Web, alors qu'elle n'apparaît dans aucun bloc ni aucune formation.

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

### Catégories

1. Je renomme une catégorie. Il faut l'exporter, et exporter tous les articles liés à la catégorie pour faire correspondre le chemin.
2. Je déplace une catégorie. Il faut l'exporter, exporter toute la descendance, et exporter tous les articles liés à la catégorie ou à la descendance. 
3. Je change le chemin d'une catégorie utilisée dans un élément de menu, il faut exporter la catégorie et le menu.

### Personnes

1. J'ajoute un bloc "chapitre" à une personne, avec une image. Il faut que cette image soit exportée pour tous les sites liés
2. J'ajoute un bloc "organisations" à une personne, avec une organisation liée. Il faut que l'organisation et son logo soient exportés pour tous les sites liés

### Formations

1. Je définis un site comme site d'école. Il faut que les formations de l'école soient exportés dans le site
2. J'ajoute un bloc "formations" dans un site. Il faut restreindre les formations proposées aux formations liées au site
3. J'ajoute un enseignant à une formation. Il faut que l'enseignant et la personne soient exportés dans tous les sites liés
4. J'ajoute un bloc "organisations" pour lister des partenaires. Il faut que les partenaires et leurs logos soient exportés vers tous les sites liés.
