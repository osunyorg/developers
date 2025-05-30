---
title: Variables SASS
---

## Layout

Variables associées : 

```sass
$grid-gutter: 60px
$grid-max-width: 1980px
$grid-breakpoints: (xs: 0, sm: 576px, md- 768px, lg: 992px, xl: 1200px, xxl: 1400px)
```

Pour placer un container (goutières latérales et max-width)

```sass
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

```sass
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

```sass
@function col($nb, $base: 12)
    $nb: $nb/$base * 12
    $nbCol: calc( (100% + #{$grid-gutter}) / 12 * #{$nb} )
    @return #{$nbCol}
```

> Faut-il aussi gérer le breakpoint dans le mixin ?

## Reset

Création d'un mixin **button-reset** qui efface les propriétés par défaut des boutons :

```sass
 appearance: none
    background: transparent
    border: none
    border-radius: 0
    cursor: pointer
    user-select: none
    &:active, &:focus
        box-shadow: 0 0 0 0.25rem rgba($main-color, 0.25)
        outline: 0
```

> Enjeu pour la transcription : Est-ce que cela restera un bouton ? Possibilité d'utiliser ```<details>``` pour le collapse.

Même principe pour enlever le style par défaut des listes :

```sass
@mixin list-reset
    list-style: none
    padding-left: 0
    margin-bottom: 0
    margin-top: 0
```

## Misc

Création d'un mixin permettant de paraméter l'équivalent de ```inset``` pour la rétrocompatibilité :

```sass
@mixin inset($top: 0, $right: $top, $bottom: $top, $left: $top)
    inset: $top $right $bottom $left
    // polyfill for inset
    @supports not (inset: $top)
        bottom: $bottom
        left: $left
        right: $right
        top: $top
```

Création d'un mixin permettant de reproduire le fonctionnement bootstrap du stretched-link des cards :

```sass
@mixin stretched-link($pseudo-element: after)
    &::#{$pseudo-element}
        bottom: 0
        content: ''
        left: 0
        position: absolute
        right: 0
        top: 0
        z-index: $zindex-stretched-link
```

Utilisé principalement pour masquer les ```<caption>```, le mixin visually-hidden permet de faire en sorte de dissimuler un élément lisible par les technologie d'assistance.

```sass
@mixin visually-hidden
    clip: rect(0,0,0,0) !important
    border: 0 !important
    height: 1px !important
    margin: -1px !important
    overflow: hidden !important
    padding: 0 !important
    position: absolute !important
    white-space: nowrap !important
    width: 1px !important
```
