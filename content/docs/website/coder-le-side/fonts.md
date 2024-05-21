---
title: Fonts
weight: 5
---

Comment modifier les polices du site ?

## Ajouter les fichiers des polices

Placer les fichiers dans `static/assets/fonts`. Favoriser les formats `woff` pour une bonne retro-compatibilité [caniuse.com/woff](https://caniuse.com/woff) et `woff2` pour la réduction du poids [caniuse.com/woff2](https://caniuse.com/woff2).

## Charger les polices

Dans le fichier `assets/sass/_fonts.sass` (voir ["Organiser ses fichiers"](/docs/website/coder-le-side/#organiser-ses-fichiers)), utiliser le mixin `font-face`.

```sass {filename="assets/sass/_fonts.sass"}
@mixin font-face($name, $path, $weight: 400, $style: normal, $exts: (eot woff2 woff ttf svg))
```

| Paramètres | @font-face CSS | Détail | Exemple |
| -------- | -------------- | ------ | ------- |
| $name    | `font-family`  | Nom de la famille de police     | 'Times New Roman' |
| $path    | `src`          | Nom des fichiers sans extension | 'times-new-roman-bold' |
| $weight  | `font-weight`  | Graisse                         | '700' |
| $style   | `font-style`   | Style                           | 'normal' |
| $exts    |                | Tableau des différents extension des formats fournis | '(eot woff2 woff)' |


Exemple d'utilisation :

```sass {filename="assets/sass/_fonts.sass"}
@include font-face("Times New Roman", times-new-roman, $exts: (woff2 woff))
@include font-face("Times New Roman", times-new-roman-bold, $weight: 700, $exts: (woff2 woff))
@include font-face("Times New Roman", times-new-roman-italic, $style: italic, $exts: (woff2 woff))
@include font-face("Times New Roman", times-new-roman-bold-italic, $weight: 700, $style: italic, $exts: (woff2 woff))

@include font-face("Helvetica Neue", helvecita-neue, $exts: (woff2 woff))
```

## Configurer les polices pour le site

Voici un exemple pour utiliser la Times New Roman pour la titraille et la Helvetica pour le corps de texte.

```sass {filename="assets/sass/_configuration.sass"}
$heading-font-family: "Times New Roman", serif
$body-font-family: "Helvetica Neue", sans-serif
```
