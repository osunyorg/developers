---
title: "Configuration SASS"
description: >
  Fichier de configuration SASS
---

# Configuration

## Couleurs

Pour définir les couleurs principales du thème :
```(sass)
// TODO: ranger
$primary: #000000 !default
$secondary: #666666 !default
$black: #000000 !default
$light: #f8f9fa !default
```
Pour définir la couleur du texte général et la couleur de fond du site :
```(sass)
$body-color: $main-color !default
$body-background-color: $main-background-color !default
```

Pour définir la couleur des liens :
```(sass)
$link-color: $main-color !default
```

## Typography

### Font family

```(sass)
// Fonts family
// TODO: choisir typo system par défaut
$body-font-family: "Georgia", serif !default
$heading-font-family: "Helvetica", "Arial", sans-serif !default
```

### Font size, weight et height

#### Générales

##### Font sizes
```(sass)
// Fonts sizes
$body-font-size: 16px !default
$small-font-size: 14px !default
// $font-size-base: 1rem
```

##### Line height

```(sass)
// Global
$line-height-base: 1.4

// Headings
$h1-line-height: 120 !default
$h2-line-height: 110 !default
$h3-line-height: 110 !default
$h4-line-height: 130 !default
```

##### Font-weight

```(sass)
$h1-weight: $heading-font-weight !default
$h2-weight: $heading-font-weight !default
$h3-weight: $heading-font-weight !default
$h4-weight: $heading-font-weight !default
$h5-weight: $heading-font-weight !default
$h6-weight: $heading-font-weight !default
```

#### Headings sizes

Le site étant codé en mobile-first, les apparences des titres sont par défaut définies pour correspondre au mobile.

##### Mobile

```(sass)
$h1-size: px2rem(30) !default
$h2-size: px2rem(40) !default
$h3-size: px2rem(30) !default
$h4-size: px2rem(20) !default
$h5-size: px2rem(18) !default
$h6-size: px2rem(16) !default
```

##### Desktop

```(sass)
$h1-size-md: px2rem(60) !default
$h2-size-md: px2rem(40) !default
$h3-size-md: px2rem(30) !default
$h4-size-md: px2rem(20) !default
$h5-size-md: px2rem(18) !default
$h6-size-md: px2rem(16) !default
```

## Grid et espacements

```(sass)
// Spacing
$spacing1: 24px !default
$spacing2: 48px !default
$spacing3: 64px !default
$spacing4: 128px !default
$spacing5: 256px !default

// Grid
$grid-gutter: 60px
$grid-max-width: 1980px
```

## Z-index

Utile pour la navigation accessible masquée destinée aux technologies d'assistance, ainsi que pour les liens des cards.

```(sass)
$zindex-nav-accessibility: 1010 !default
$zindex-stretched-link: 2 !default
```

## Éléments du design-system

### Breadcrumb

```(sass)
// Breadcrumb
$breadcrumb-color: invert($main-color) !default
```

### Header

Les couleurs du header sont personnalisables :
```(sass)
$header-color: $main-color !default
$header-hover-color: rgba($header-color, 0.7) !default // TODO : Réflechir à plus élégant / générique
$header-background-color: $main-background-color !default
```

L'animation du header (sticky) et des dropdowns est paramétrable :

```(sass)
$header-sticky-enabled: true !default
$header-transition: 0.3s !default
$header-sticky-transition: $header-transition !default
$header-dropdown-transition: $header-transition !default
```

Il est possible de personnaliser la hauteur du header de navigation, sur desktop et mobile :

```(sass)
// Mobile
$header-height: 61px !default

// Desktop
$header-height-md: 74px !default
```

### Footer

```(sass)
$footer-color: $main-color !default
$footer-background-color: darken($main-background-color, 2.5) !default
```

### Hero

```(sass)
$hero-height: 375px !default
$hero-height-md: 450px !default
$hero-color: invert($main-color) !default
$hero-background-color: invert($main-background-color) !default
```

### Icons

```(sass)
$icons: ()
$icons: map-merge($icons, ("arrow": "\e905"))
$icons: map-merge($icons, ("arrow-first": "\e906"))
$icons: map-merge($icons, ("arrow-last": "\e907"))
$icons: map-merge($icons, ("arrow-left": "\e908"))
$icons: map-merge($icons, ("arrow-right": "\e909"))
$icons: map-merge($icons, ("burger": "\e902"))
$icons: map-merge($icons, ("caret": "\e904"))
$icons: map-merge($icons, ("caret-bottom": "\e911"))
$icons: map-merge($icons, ("caret-left": "\e912"))
$icons: map-merge($icons, ("caret-right": "\e913"))
$icons: map-merge($icons, ("caret-top": "\e914"))
$icons: map-merge($icons, ("close": "\e90e"))
$icons: map-merge($icons, ("download": "\e900"))
$icons: map-merge($icons, ("eye": "\e901"))
$icons: map-merge($icons, ("link-blank": "\e903"))
$icons: map-merge($icons, ("play": "\e910"))
$icons: map-merge($icons, ("pause": "\e90f"))
$icons: map-merge($icons, ("social": "\e915"))
$icons: map-merge($icons, ("instagram": "\e90a"))
$icons: map-merge($icons, ("facebook": "\e90b"))
$icons: map-merge($icons, ("linkedin": "\e90c"))
$icons: map-merge($icons, ("twitter": "\e90d"))
```

### Breakpoints

```(sass)
// TODO: réécrire en sass les mixins bootstrap
$grid-breakpoints: (xs: 0, sm: 576px, md: 768px, lg: 992px, xl: 1200px, xxl: 1400px)
```

## BLOCKS

### Block call to action
```(sass)
$block-call-to-action-background: $main-background-color !default
$block-call-to-action-color: $main-color !default
```

### Block pages

```(sass)
// Layout cards
$block-pages-card-background: lighten($main-background-color, 1) !default
$block-pages-card-page-background: white !default
$block-pages-card-page-color: $main-color !default
$block-pages-card-page-background-hover: lighten($main-background-color, 2) !default
$block-pages-card-page-color-hover: white !default
```

### Block testimonials

Paramètres par défaut :
```(sass)
$block-testimonials-font-size: px2rem(24) !default
$block-testimonials-line-height: px2rem(28.8) !default
$block-testimonials-pagination-background: $main-border-color !default
$block-testimonials-pagination-progress-background: $main-color !default
```

Pour les grands écrans :
```(sass)
$block-testimonials-xl-font-size: px2rem(50) !default
$block-testimonials-xl-line-height: px2rem(55) !default
$block-testimonials-xl-font-size-long-text: px2rem(36) !default
$block-testimonials-xl-line-height-long-text: px2rem(39.6) !default
```

### Block timeline
```(sass)
$block-timeline-horizontal-background: black !default
$block-timeline-horizontal-color: white !default
```

## Sections
```(sass)
$post-media-background: darken($main-background-color, 2) !default
$post-media-aspect-ratio: 50%
$post-categories-color: lighten($main-color, 2) !default
$post-time-color: lighten($main-color, 2) !default
```

### Widgets

```(sass)
$widget-media-background: darken($main-background-color, 2) !default
$widget-title-size: $h4-size !default
$widget-title-size-md: $h4-size-md !default
```

### Table of content

```(sass)
$toc-color: $main-color !default
$toc-active-color: $main-color !default // TODO : checker ce que ça fait
```

### Program

Font-size du cadre ```.essential``` :

```(sass)
$program-essential-font-size: $h5-size !default
$program-essential-font-size-md: $h5-size-md !default
```

Font-size du bouton de partage d'une formation :
```(sass)
$program-share-font-size: $h4-size !default
$program-share-font-size-md: $h4-size-md !default
```

Paramétrage du z-index de l'aside horizontal et sticky :
```(sass)
$program-zindex-toc: $zindex-toc !default
```

## MISC

### Animations
```(sass)
$arrow-ease-transition: cubic-bezier(0, 0.65, 0.4, 1.2) !default
$arrow-ease-transition-2: cubic-bezier(0, 0.65, 0.4, 1) !default
```