---
title: Principes de fonctionnement
weight: 1
description: >
  Comment fonctionnent les blocs ?
---


# Comportement back

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

# Comportement front

Les contenus des blocs sont encapsulé dans un container. Cela permet une gestion autonome de la grille au sein des blocs.

Il existe dans le thème par défaut 2 types de layout : une page en pleine largeur et une page contenant un aside qui réduit la largeur du contenu à 8 colonnes.

Une bodyclass permet de faciliter l'affichage des blocks, "content-aside" and "content-full".

Par exemple, un bloc chapitre devra s'afficher sur 8 colonnes dans une page pleine largeur, et sur toute la largeur disponible dans une page contenant un aside.


## Roadmap

1. Block pages : modifier la clé "slug" des pages enfant en "page". Modifier le static généré côté osuny, modifier le thème (remplacer slug par page), modifier les fichiers statics de legacy dans les sites existants
2. Homogénéiser les images, structure à suivre :
  ```
    image:
      id:
      alt: >-
        Texte alternatif de l'image
      credit: >-
        Crédit de l'image
  ```
