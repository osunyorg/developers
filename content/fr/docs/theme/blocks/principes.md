---
title: Principes de fonctionnement
weight: 1
description: >
  Comment fonctionnent les blocs ?
---

## Types de pages

Les contenus des blocs sont encapsulé dans un container. Cela permet une gestion autonome de la grille au sein des blocs.

Il existe dans le thème par défaut 2 types de layout : une page en pleine largeur et une page contenant un aside qui réduit la largeur du contenu à 8 colonnes.

Une bodyclass permet de faciliter l'affichage des blocks, "content-aside" and "content-full".

Par exemple, un bloc chapitre devra s'afficher sur 8 colonnes dans une page pleine largeur, et sur toute la largeur disponible dans une page contenant un aside.

## Titres

Tous les titres sont balisés par défaut en h2. 
En revanche, dans les programmes, les blocs s'intègrent dans la partie présentation, comme des sous-parties.
Il faut donc baliser les titres en h3.

Afin d'obtenir ce comportement, l'idée est de pouvoir passer un niveau (level) en contexte aux blocs, qui est par défaut à 1.
Quand le niveau est 1, les titres sont des h2, et les sous-titres des h3 (par exemple dans les timelines ou dans les listes de personnes).
Quand le même bloc est dans un programme, le niveau est 2 donc les titres sont des h3 et les sous-titres des h4.


Cas particuliers : 
- en desktop pleine page, tous les titres sont stylisés comme des sections (h5)
- dans le block pages, les titres stylisés en h5 en mobile, car ils sont suivis d'un texte stylisé en h2

## Données

Tous les blocs possèdent un titre, un data en JSONB et une position.

Les attributs dans le JSONB côté Osuny peuvent avoir un type parmi :
* `string` : Champ texte classique
* `text` : Champ textarea
* `richtext (<config>)` : Richtext Summernote avec une configuration définie (`mini`, `mini-list` ou `default`)
  * exemple : `richtext (mini-list)`
* `integer` : Champ de nombre entier
* `float` : Champ de nombre décimal
* `boolean` : Case à cocher (vrai/faux)
* `enum (<value1>, <value2>[, ...])` : Enumération de valeurs possibles
* `references (<model>)` : UUID faisant référence à un objet avec le modèle associé
  * exemple : `references (Communication::Website::Page)`
* `blob` : Champ d'upload de fichier (pour les images notamment), représenté par un objet ayant des attributs `id`, `filename` et `signed_id`
* `array` : Pour un tableau d'éléments
* `hash` : Pour un objet avec des paires clé-valeur


Les attributs dans le YAML statique côté Hugo peuvent avoir un type parmi :
* `string` : Texte classique
* `text` : Texte avec retours à la ligne
* `richtext` : Richtext Summernote
* `integer` : Nombre entier
* `float` : Nombre décimal
* `boolean` : Booléen (vrai/faux)
* `references (<model>)` : Référence à un objet avec le modèle associé
  * Pour une catégorie ou une page, il s'agit du `path`
  * Pour un post, il s'agit du `slug`
  * Pour un blob, il s'agit de l'`uuid`
* `image` : Un objet ayant des attributs `id`, `alt` et `credit` pour représenter une image situé dans le dossier `data/medias`
* `array` : Pour un tableau d'éléments
* `hash` : Pour un objet avec des paires clé-valeur

