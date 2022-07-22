---
title: "Utils"
description: >
---

## Layout

Variables associées : 

```
$grid-gutter: 60px
$grid-max-width: 1980px
$grid-breakpoints: (xs: 0, sm: 576px, md: 768px, lg: 992px, xl: 1200px, xxl: 1400px)
```

Pour placer un container (goutières latérales et max-width)

```
@mixin container
    max-width: $grid-max-width
    padding-left: $grid-gutter
    padding-right: $grid-gutter
    margin-left: auto
    margin-right: auto
    width: 100%
```


Usage des colonnages avec CSS Grid.

Utilise le mixin **grid**, avec le nombre de colonnes voulues, et à quel breakpoint. Par défaut, pas de grid sous le breakpoint **md**

```
@mixin grid($cols: 12, $breakpoint: md)
    @include media-breakpoint-up($breakpoint)
        display: grid
        grid-gap: 0 $grid-gutter
        grid-template-columns: repeat($cols, 1fr)
```

> Problème : il faut mettre les appels avec des breakpoints dans l'ordre croissant de largeur. 
> Faut-il vraiment gérer le breakpoint dans le mixin ?
> Par exemple, on veut 3 posts côte à côte en **xl**, et 2 en **md** :
> ```
> .posts
>    @include grid(2)
>    @include grid(3, xl)
> ```


Pour assigner une valeur (width, padding...) d'un nombre de colonnes, on utilise le mixin **col**

```
@function col($nb, $base: 12)
    $nb: $nb/$base * 12
    $nbCol: calc( (100% + #{$grid-gutter}) / 12 * #{$nb} )
    @return #{$nbCol}
```

> Faut-il aussi gérer le breakpoint dans le mixin ?

Création d'un mixin **button-reset** qui efface les propriétés par défaut des boutons :

```
@mixin button-reset
    background: transparent
    border: none
    cursor: pointer
    &:active, &:focus
        box-shadow: 0 0 0 0.25rem rgba($main-color, 0.25)
        outline: 0
```

> Enjeu pour la transcription : Est-ce que cela restera un bouton ? Possibilité d'utiliser ```<details>``` pour le collapse.