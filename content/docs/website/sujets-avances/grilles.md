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

### Mixin flexbox-grid

```
@mixin flexbox-grid($cols: 12, $gap-y: $grid-gutter, $gap-x: $gap-y)
    display: flex
    flex-wrap: wrap
    gap: $gap-y $gap-x
    > *
        flex: 0 0 calc(#{100 / $cols}% - #{$gap-x} * #{($cols - 1) / $cols} )
```

