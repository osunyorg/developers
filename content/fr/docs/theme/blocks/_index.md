---
title: "Blocs"
description: >
  Liste des blocs disponibles
---

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
      alt: >
      credit: >
  ```
