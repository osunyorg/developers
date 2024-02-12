---
title: Les grilles
description: >-
  Comment créer et utiliser des grilles dans le thème
---

## Les différents outils

### Mixin grid

```
@mixin grid($cols: 12, $breakpoint: false, $gap-y: $grid-gutter, $gap-x: $grid-gutter)
    word-break: break-word
    @if $breakpoint
        @include media-breakpoint-up($breakpoint)
            display: grid
            grid-gap: $gap-y $gap-x
            grid-template-columns: repeat($cols, 1fr)
    @else
        display: grid
        grid-gap: $gap-y $gap-x
        grid-template-columns: repeat($cols, 1fr)
    @include media-breakpoint-down(desktop)
        grid-gap: $grid-gutter-sm
```

## Correctif navigateur

### Safari : Mixin flexbox-grid

Safari ne supporte pas l'alignement baseline dans une grid CSS ```align-items: baseline```.

Pour gérer l'alignement baseline sur Safari on remplace la grille utilisant CSS Grid par une grille générée par une flexbox. 

```
@mixin flexbox-grid($cols: 12, $gap-y: $grid-gutter, $gap-x: $gap-y)
    display: flex
    flex-wrap: wrap
    gap: $gap-y $gap-x
    > *
        flex: 0 0 calc(#{100 / $cols}% - #{$gap-x} * #{($cols - 1) / $cols} )
```

$cols : nombre de colonnes attendues
$gap-y : épaisseur de la goutière verticale (64px)
$gap-x : épaisseur de la goutière horizontale (64px)


```
display: flex
flex-wrap: wrap
```

On utilise du flex et du wrap pour que ça revienne à la ligne.


On met grow à 0 et shrink à 0 pour que les enfants de l'élément "flex" fassent exactement la largeur d'une colonne.

Pour diviser en parts égales (nombre de colonnes) on a besoin de préciser la valeur du flex-basis par le calcul suivant : 



#### Problème de départ :

Les blocs galerie sur Safari en grid avec un align-items: baseline.

<img width="939" alt="Capture d’écran 2024-02-12 à 12 10 36" src="https://github.com/noesya/osuny-developers/assets/4630530/2de1c8b2-3767-4e6c-8f06-d597fcf99db5">

<img width="1440" alt="Capture d’écran 2024-02-12 à 12 10 50" src="https://github.com/noesya/osuny-developers/assets/4630530/2867a249-e730-4b64-b5d6-0d9abebc8ba2">



