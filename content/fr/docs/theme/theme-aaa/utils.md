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


```
@mixin grid($cols: 12, $breakpoint: md)
    @include media-breakpoint-up($breakpoint)
        display: grid
        grid-gap: 0 $grid-gutter
        grid-template-columns: repeat($cols, 1fr)

@function col($nb, $base: 12)
    $nb: $nb/$base * 12
    $nbCol: calc( (100% + #{$grid-gutter}) / 12 * #{$nb} )
    @return #{$nbCol}
```
