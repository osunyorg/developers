---
title: Grille
weight: 4
description: >
  Comprendre la grille de base
---

TODO expliquer pourquoi on utilise une grille
TODO expliquer les enjeux de responsivité liés à la grille

## Les spacings

Le thème prévoit 6 espacements différents, calculés en REM à partir de multiple de 12px. Nous utilisons des valeurs en REM de façon à conserver les proportions d'espacements entre les éléments peut importe les préférences d'affichage de la taille de texte de l'utilisateur.

> Questionnement 
> Comment préciser l'impact sur le design system de changement de ces valeurs ?

```
$spacing0: px2rem(12) !default
$spacing1: px2rem(24) !default
$spacing2: px2rem(48) !default
$spacing3: px2rem(64) !default
$spacing4: px2rem(128) !default
$spacing5: px2rem(256) !default
```

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


## Usage des unités

On favorise l'usage de REM pour l'adaptation au préférences utilisateurs.

### Utiliation des ex

On utilise à certains endroit l'unité ex ("La hauteur d'x de la police de l'élément." https://developer.mozilla.org/fr/docs/Learn/CSS/Building_blocks/Values_and_units ). Cela permet de gérer les alignements verticaux en fonction de la taille de caractère.

![image](https://user-images.githubusercontent.com/4630530/200541647-1c93d98a-6dc8-4cc7-b637-0b846d1cc353.png)
