---
title: "Configuration"
description: >
  Configuration des variables sass d'un site
---

## Configuration par défaut

Le fichier de configuration par défaut se trouve themes/osuny-hugo-theme/assets/sass/_theme/_default_config.sass

## Description des variables


#### Icons

Les icônes sont stockée dans un tableau sass, de façon à faciliter l'usage des mixins.


```(sass)
// _theme/_default_config.sass
$icons: ()
$icons: map-merge($icons, ("arrow": "\e905"))
```

Usage 

```(sass)
// _theme/_default_config.sass
&::after
    @include icon
    content: map-get($icons, "caret")
```

### Blocks

#### Block call to action

Permet de changer la couleur de fond

```(sass)
// block call to action
$block-call-to-action-background: $primary !default
```

Permet de changer la couleur du texte

```(sass)
// block call to action
$block-call-to-action-color: white !default
```

#### Block pages

##### Layout cards

Permet de changer la couleur de fond du block

```(sass)
// block pages layout cards
$block-pages-card-background: #F8F9FA !default
````

Permet de changer les couleurs des cartes

```(sass)
// le fond d'une carte page
$block-pages-card-page-background: white !default
// la couleur du texte des cartes
$block-pages-card-page-color: $primary !default
````

Permet de changer les couleurs des cartes au survol

```(sass)
$block-pages-card-page-background-hover: $primary !default
$block-pages-card-page-color-hover: white !default
```

##### Layout list

Permet de changer la transition de l'animation des flèches

```(sass)
$arrow-ease-transition: cubic-bezier(0, 0.65, 0.4, 1.2)
$arrow-ease-transition-2: cubic-bezier(0, 0.65, 0.4, 1)
```

#### Block timeline

##### Layout horizontal

Permet de changer la couleur de fond du block

```(sass)
$block-timeline-horizontal-background: black !default
```

Permet de changer la couleur du texte

```(sass)
$block-timeline-horizontal-color: white !default
```

### Layouts & partials

Header
```(sass)
$header-background-color: white !default
$header-color: black !default
$header-hover-color: $header-color !default
```

Hero
```
$hero-background-color: $primary !default
$hero-color: white !default
```

Nav : navigation menu
```(sass)
$menu-background-color: white !default
```

Toc : table of content 
```(sass)
$toc-height: 78px
```

Footer 
```
$footer-background: $light !default
$footer-color: $black !default
```
