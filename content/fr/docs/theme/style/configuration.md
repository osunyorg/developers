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

#### Block pages

Permet de changer la couleur de fond

```(sass)
// block pages
$block-pages-card-background: #f8f9fa !default
```

### Layouts & partials

Toc : table of content 
```(sass)
$toc-height: 78px
```