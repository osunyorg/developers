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

## Définitions

### Objet direct
Un objet connecté directement à un site Web, comme une page ou une actualité.

À noter, le site Web lui-même n'est pas strictement un objet direct, mais peut néanmoins servir de source à un objet indirect, par sa propriété `about`.

### Objet indirect
Un objet connecté à un site Web par le biais d'un objet direct.
Les objets indirects peuvent se connecter entre eux, et ils héritent de la connexion à l'objet direct.
Un objet indirect peut être connecté à plusieurs objets directs, au sein du même site ou de plusieurs sites.

### Dépendances
L'ensemble des objets liés à un autre et nécessaires à son affichage dans le site.

Par exemple, sont des dépendances d'une actualité :
- son image à la une
- son auteur
- la photo de l'auteur (si elle est affichée sur la page de l'actualité)

### Références
L'ensemble des objets qui font référence à l'objet courant, en utilisant son `path`, son `slug`.

Par exemple :
- une actualité est référencée par l'ensemble des blocs actualité (même si la réalité est un peu plus subtile)
- une page est référencée par son parent (dans les children)

### Connexions
On appelle "connexions" l'ensemble des liens entre un site Web et des objets indirects qui apparaissent sur ce site.
La connexion est toujours une jointure entre un objet direct `direct_source` et un objet indirect `indirect_object`.
On distingue connexions et dépendances pour clarifier la réflexion.
Les dépendances mêlent indistinctement objets directs et indirects.
Les connexions sont exclusivement des objets indirects.

À noter, la plupart des connexions concernent des objets ActiveRecord persistés en base de données, mais pas tous.
Les composants et templates de blocs, par exemple, ne sont pas des objets persistés en BDD.

### Événements déclencheurs
Certains événements (création, suppression, lien...) déclenchent certaines mises à jour de contenus.
Pour éviter les calculs inutiles, le lien entre événements déclencheurs et dépendances doit être précis.

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
