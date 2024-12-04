---
title: Comprendre les blocs
weight: 4
---

*Les blocs sont les briques de base d'Osuny, ils font le pont entre l'admin et le thème*


![](/images/home/blocks.jpg)

## Contextes

Les contenus (pages, actualités...) peuvent être pleine largeur ou non, ce qui change leur mise en page desktop.
Chaque bloc fonctionne donc dans 4 contextes :
- desktop pleine largeur
- desktop avec barre latérale
- tablette
- mobile

Lorsque la page est en pleine largeur, le body est doté de la classe `full-width`.

Les mixins suivants permettent d'adresser les contextes différents :
- `@include in-page-without-sidebar` ou `@include full-page` pour les pages pleine largeur
- `@include in-page-with-sidebar` ou `@include not-full-page` pour les pages avec barre latérale
- `@include in-page-with-or-without-sidebar`pour adresser les 2 cas (TODO expliquer pourquoi ça et pas rien ?)

| Périphérique | Contexte | Directive
|-|-|-
| mobile | | `@include media-breakpoint-down(mobile)`
| desktop | | `@include media-breakpoint-up(desktop)`
| desktop | pleine largeur | `@include in-page-without-sidebar` ou `@include full-page`
| desktop | avec barre latérale | `@include in-page-with-sidebar` ou `@include not-full-page`

## Titres

Les blocs maintiennent une hiérarchie de titre juste de façon autonome, en fonction des contributions. Voir à ce sujet [Accessibilité numérique : un bon plan](https://lab.noesya.coop/publications/2023-12-07-accessibilite-numerique-un-bon-plan/).


## Formats de données

Les attributs dans le YAML utilisent des types.


| Type | Description
|-|-
| `string` | Texte classique
| `text` | Texte avec retours à la ligne
| `richtext` | Richtext Summernote
| `integer` | Nombre entier
| `float` | Nombre décimal
| `boolean` | Booléen (vrai/faux)
| `objet` | Référence à un objet |
| `blob` | Le `uuid`
| `image` | Un objet ayant des attributs `id`, `alt` et `credit` pour représenter une image situé dans le dossier `data/medias`
| `array` | Pour un tableau d'éléments
| `hash` | Pour un objet avec des paires clé-valeur

### Référence à un objet

#### `permalink`

La vraie url, sans le domaine. Avec ou sans langue.

Exemples :
- /actualites/2024-05-17-lapplication-osuny-presente-le-niveau-de-securite-bon/

#### `path`

L'identifiant qu'utilise Hugo.

La [page de documentation](https://gohugo.io/methods/page/path/) explique la méthode, qui s'appuie sur le chemin du fichier physique :
1. Enlever l'extension de fichier
2. Enlever la langue
3. Passer en bas de casse
4. Remplacer les espaces par des tirets

On remarque qu'il n'y a pas non plus mention du dossier `content`, il est implicite.

Exemples :
- /posts/2024/2024-05-17-lapplication-osuny-presente-le-niveau-de-securite-bon
- /events/2024-12-27-communs-numerique-et-interet-general

#### `slug`

L'identifiant unique de l'objet, tel quel.
Les slugs semblent être les identifiants à utiliser dans les taxonomies Hugo, avec des slash à l'intérieur pour les taxonomies et les catégories en arbre.

Exemples :
- lapplication-osuny-presente-le-niveau-de-securite-bon
- communs-numerique-et-interet-general
- taxonomie/taxon-1

#### `file`

Le chemin du fichier physique dans le référentiel Git.

Exemples :
- content/fr/posts/2024/2024-05-17-lapplication-osuny-presente-le-niveau-de-securite-bon.html


### Référence à un blob

TODO expliquer la mécanique

## Les blocs qui n'existent pas

### Carrousel

Problèmes d'efficacité et d'accessibilité : 
- https://www.doisjeutiliser.fr/unCarrousel/
- https://siecledigital.fr/2014/07/31/carrousel-abandon-ecommerce/
- https://www.pragm.co/article/carrousels-marketing-web
- https://www.wenovio.com/2021/04/27/carrousel-web-les-avantages-et-inconvenients/
- https://www.orbitmedia.com/blog/rotating-sliders-hurt-website/
- https://www.testapic.com/informations-pratiques/actualites/best-practices/exploration-usage-carrousel-sites-web-mobile-ecommerce/
- https://yumea.fr/blog/8-tendances-obsoletes-webdesign/