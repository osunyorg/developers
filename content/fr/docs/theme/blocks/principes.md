---
title: Principes de fonctionnement
weight: 1
description: >
  Comment fonctionnent les blocs ?
---

## Contextes

Les contenus (pages, actualités...) peuvent être pleine largeur ou non, ce qui change leur mise en page desktop.
Chaque bloc fonctionne donc dans 3 formats :
- desktop pleine largeur
- desktop avec barre latérale
- mobile

*Par exemple, un bloc chapitre devra s'afficher sur 8 colonnes dans une page pleine largeur, et sur toute la largeur disponible dans une page contenant un aside.*


Lorsque la page est en pleine largeur, le body est doté de la classe `full-width`.

Les mixins suivants permettent d'adresser les contextes différents :
- `@include in-page-without-sidebar` ou `@include full-page` pour les pages pleine largeur
- `@include in-page-with-sidebar` ou `@include not-full-page` pour les pages avec barre latérale
- `@include in-page-with-or-without-sidebar`pour adresser les 2 cas (TODO expliquer pourquoi ça et pas rien ?)

| Périphérique | Contexte | Directive
|-|-|-
| mobile | | `@include media-breakpoint-down(md)`
| desktop | | `@include media-breakpoint-up(md)`
| desktop | pleine largeur | `@include in-page-without-sidebar` ou `@include full-page`
| desktop | avec barre latérale | `@include in-page-with-sidebar` ou `@include not-full-page`

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

Les attributs dans le YAML utilisent des types.


| Type | Description
|-|-
| `string` | Texte classique
| `text` | Texte avec retours à la ligne
| `richtext` | Richtext Summernote
| `integer` | Nombre entier
| `float` | Nombre décimal
| `boolean` | Booléen (vrai/faux)
| `references (<model>)` | Référence à un objet avec le modèle associé : pour une catégorie ou une page, il s'agit du `path`, pour un post, il s'agit du `slug`, pour un blob, il s'agit de l'`uuid`
| `image` | Un objet ayant des attributs `id`, `alt` et `credit` pour représenter une image situé dans le dossier `data/medias`
| `array` | Pour un tableau d'éléments
| `hash` | Pour un objet avec des paires clé-valeur

