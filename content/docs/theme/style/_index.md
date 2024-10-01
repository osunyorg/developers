---
title: Style
---

Comment le thème est-il intégré.

## Où placer les styles conditionnelles dans le code (@media et layout de page) ?

### Approche global / local 

1. Le style commun à tous les contexte
2. Les modificateurs simples
3. Le mobile
4. Le desktop
5. Le layout de la page (avec ou sans sidebar)

```sass
.block-chapter
    p:last-child
        margin-bottom: 0
    .notes
        @include small
        margin-top: $spacing-3
        sub, sup
            font-size: 60%
            margin-left: 0
    [...]
    &--alt_background
        background: $block-chapter-layout-alt-background
        .block-content
            color: $block-chapter-layout-alt-color
    &--accent_background
        background: $block-chapter-layout-accent-background
        .block-content
            color: $block-chapter-layout-accent-color

    @include media-breakpoint-down(desktop)
        &--alt_background,
        &--accent_background
            padding-top: var(--grid-gutter)
            padding-bottom: var(--grid-gutter)

    @include in-page-with-sidebar
        figure
            max-width: columns(6)
            &.image-portrait,
            &.image-square
                max-width: columns(4)
        [...]

    @include in-page-without-sidebar
        &--alt_background, 
        &--accent_background
            [...]
```

Si les modificateurs comme les layouts de bloc, on descend d'un niveau pour plus de lisibilité : 

1. Le style commun à tous les contexte
2. Les layouts de blocs
  a. Le style commun aux contextes
  b. Le mobile
  c. Le desktop
  d. Le layout de la page (avec ou sans sidebar)


```sass
.block-posts
    .top
        margin-bottom: $spacing-4
        a
            @include icon(arrow-right-line, after, true)
    .posts
        @include grid($block-posts-grid-columns, desktop)
    [...]

    &--grid
        @include media-breakpoint-down(desktop)
            article + article
                margin-top: $spacing-5
        @include in-page-with-sidebar
            .grid
                @include grid(2)
        @include in-page-without-sidebar
            .grid
                @include grid($block-posts-grid-columns)
            [...]

    &--large
        .post
            .more
                @include icon(arrow-right-line, after, true)
        @include media-breakpoint-down(desktop)
            .post
                [...]
        @include media-breakpoint-up(desktop)
            .large
                .post
                    [...]
        @include in-page-with-sidebar
            .large
                .post
                    [...]
        @include in-page-without-sidebar
            .large
                .post
                    [...]

    &--list
        [...]

    &--highlight
        [...]

    &--alternate .alternate
        [...]

    &--carousel
        [...]

```

### Approche micro

On insère les medias dans les sélecteurs profonds pour limiter de dupliquer des sélecteurs longs : 

```sass
.block-key_figures
    dl
        dt
            font-family: $block-key_figures-number-font-family
            font-size: $block-key_figures-font-size
            font-weight: $block-key_figures-unit-font-weight
            [...]
            @include media-breakpoint-up(desktop)
                font-size: $block-key_figures-font-size-desktop
                strong
                    font-size: $block-key_figures-number-font-size-desktop
                img
                    max-width: $block-key_figures-image-max-width-desktop
            @include media-breakpoint-up(lg)
                font-size: $block-key_figures-font-size-lg
                strong
                    font-size: $block-key_figures-number-font-size-lg
            @include media-breakpoint-up(xl)
                font-size: $block-key_figures-font-size-xl
                strong
                    font-size: $block-key_figures-number-font-size-xl
            @include media-breakpoint-up(xxl)
                font-size: $block-key_figures-font-size-xxl
                strong
                    font-size: $block-key_figures-number-font-size-xxl
```

Ça factorise un code qui pourrait s'écrire comme : 

```sass
.block-key_figures
    @include media-breakpoint-up(desktop)
        dl 
            dt
                font-size: $block-key_figures-font-size-desktop
                strong
                    font-size: $block-key_figures-number-font-size-desktop
                img
                    max-width: $block-key_figures-image-max-width-desktop
    @include media-breakpoint-up(lg)
        dl
            dt
                font-size: $block-key_figures-font-size-lg
                strong
                    font-size: $block-key_figures-number-font-size-lg
    @include media-breakpoint-up(xl)
        dl
            dt
                font-size: $block-key_figures-font-size-xl
                strong
                    font-size: $block-key_figures-number-font-size-xl
    @include media-breakpoint-up(xxl)
        dl
            dt
                font-size: $block-key_figures-font-size-xxl
                strong
                    font-size: $block-key_figures-number-font-size-xxl
```

## Comment ordonner les sélecteurs CSS d'un composant

On essai de suivre au mieux l'ordre du markup HTML. L'ordre HTML étant proche de l'ordre visuel des éléments (mais pas toujours : souvent on place l'image avant le texte via du CSS pour l'accessibilité), ça permet de s'appuyer aussi sur le rendu graphique pour s'y retrouver.


```html
<div class="chapter">
   <div class="text">
      <div class="rich-text">
         <p>...</p>
      </div>
      <div class="notes">
         ...
      </div>
      <figure class="image-landscape">
        <picture>
            <img src="...">
        </picture>
        <figcaption>...</figcaption>
      </figure>
   </div>
</div>
```

```sass
.block-chapter
    p:last-child
        [...]
    .notes
        [...]
        sub, sup
            [...]
    picture, img
        [...]
    figcaption
        [...]
```


## Comment ordonner les proprétés CSS

Ordre alphabétique
