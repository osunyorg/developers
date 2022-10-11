---
title: Grille
weight: 4
description: >
  Comprendre la grille de base
---

TODO expliquer pourquoi on utilise une grille
TODO expliquer les enjeux de responsivité liés à la grille

## La grille par défaut

Le plugin [CSS Grid Overlay](https://chrome.google.com/webstore/detail/css-grid-overlay/hajfilceeneohkmcakehndmaeonhlack) permet d'afficher la grille de son choix, et facilite le développement.

Configuration par défaut de la grille du Thème Osuny
```json
[
  {
    "columns": 6,
    "from": 0,
    "gutters": 10,
    "margins": 20,
    "maxWidth": 1024,
    "to": 1023
  },
  {
    "columns": 12,
    "from": 1024,
    "gutters": 30,
    "margins": 60,
    "maxWidth": 1920
  }
]
```