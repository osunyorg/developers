---
title: Configuration SASS
weight: 3
---

Le thème `osuny-hugo-theme-aaa` inclus dans le dossier `/themes/osuny` est un thème Hugo, de nombreuses variables SASS sont définis dans celui-ci. Il est donc possible d'utiliser ces variables pour personnaliser le site de différentes manières.
L'intégralité des variables SASS disponible à la modification ce trouve dans le fichier [assets/sass/\_theme/\_configuration.sass](https://github.com/osunyorg/theme/blob/main/assets/sass/_theme/_configuration.sass) du theme.

## Couleurs

Pour définir les couleurs principales du thème :

```sass
$color-accent: #0038FF !default
$color-text: #000000 !default
$color-text-alt: #454545 !default
$color-border: rgba(0, 0, 0, 0.30) !default
$color-background-alt: #F2F2F2 !default
$color-background: #FFFFFF !default
```

Pour définir la couleur du texte général et la couleur de fond du site :

```sass
$body-color: $main-color !default
$body-background-color: $main-background-color !default
```

Pour définir l'apparence des liens (couleur et espace entre le soulignement et le lien) :

```sass
$link-color: $main-color !default
$link-underline-offset: 6px !default
```

## Typographie

### Font family

```sass
/* Fonts family */
$body-font-family: "Baskerville", "Times New Roman", "Times", serif !default
$heading-font-family: "Helvetica Neue", "Helvetica", "Arial", sans-serif !default
```

### Font size, weight et height

#### Font sizes

```sass

/* Générales */
$body-size-desktop: px2rem(22) !default
$body-size: px2rem(18) !default

/* Lead */
$lead-size-desktop: px2rem(60) !default
$lead-size: px2rem(24) !default

/* Small */
$small-size-desktop: px2rem(18) !default

/* Signature */
$small-size: px2rem(14) !default
$signature-size-desktop: px2rem(22) !default
$signature-size: px2rem(18) !default

/* Meta */
$meta-size-desktop: px2rem(16) !default
$meta-size: px2rem(14) !default
```

#### Line height

```sass

/* Général */
$body-line-height: 160% !default

/* Headings */
$h1-line-height: 120 !default
$h2-line-height: 110 !default
$h3-line-height: 110 !default
$h4-line-height: 130 !default
$h5-line-height: 130% !default
$h6-line-height: 130% !default

/* Cas particuliers */
$lead-line-height: 120% !default
$small-line-height: 130% !default
$signature-line-height: 130% !default
$meta-line-height: 150% !default
$quote-line-height: 120% !default
```

#### Font-weight

```sass
$body-weight: normal !default
```

Une variable globale permet de gérer les font-weight des titres :

```sass
$heading-font-weight: normal !default
```

Chaque titre peut ensuite être personnalisé :

```sass
$h1-weight: bold !default
$h2-weight: $heading-font-weight !default
$h3-weight: bold !default
$h4-weight: bold !default
$h5-weight: $heading-font-weight !default
$h6-weight: $heading-font-weight !default
```

#### Headings sizes

Le site étant codé en mobile-first, les apparences des titres sont par défaut définies pour correspondre au mobile.

##### Mobile

```sass
$h1-size: px2rem(30) !default
$h2-size: px2rem(40) !default
$h3-size: px2rem(30) !default
$h4-size: px2rem(20) !default
$h5-size: px2rem(18) !default
$h6-size: px2rem(16) !default
```

##### Desktop

```sass
$h1-size-desktop: px2rem(60) !default
$h2-size-desktop: px2rem(40) !default
$h3-size-desktop: px2rem(30) !default
$h4-size-desktop: px2rem(20) !default
$h5-size-desktop: px2rem(18) !default
$h6-size-desktop: px2rem(16) !default
```

#### Typographies particulières

De nombreuses variables permettent de personnaliser l'affichage des différents niveaux de titres dans le site :

```sass
/* Lead */
$lead-weight: $heading-font-weight !default

/* Small */
$small-weight: normal !default

/* Signature */
$signature-weight: $heading-font-weight !default

/* Meta */
$meta-weight: $heading-font-weight !default

/* Quote */
$quote-weight: normal !default
$quote-style: italic !default
```

## Grid et espacements

```sass
/* Spacing */
$spacing1: 24px !default
$spacing2: 48px !default
$spacing3: 64px !default
$spacing4: 128px !default
$spacing5: 256px !default

/* Grid */
$grid-gutter: 60px
$grid-max-width: 1980px
```

## Z-index

Utile pour la navigation accessible masquée destinée aux technologies d'assistance, ainsi que pour les liens des cards.

```sass
$zindex-nav-accessibility: 1010 !default
$zindex-stretched-link: 2 !default
```

## Éléments du design system

### Breadcrumb

Pour personnaliser l'apparence du fil d'ariane, on peut utiliser les variables suivantes :

```sass
$breadcrumb-below-h1: false !default
$breadcrumb-color: $color-text !default
$breadcrumb-icon: "caret-right" !default
$breadcrumb-icon-color: $color-text-alt !default
```

> L'option breadcrumb-below-h1 permet de changer l'affichage du fil d'ariane, en le plaçant au-dessus ou en-dessous de la page.

### Breakpoints

```sass
/* TODO: réécrire en sass les mixins bootstrap */
$grid-breakpoints: (xs: 0, sm: 576px, md: 768px, lg: 992px, xl: 1200px, xxl: 1400px)
```

### Header

Les couleurs du header sont personnalisables :

```sass
/* Typographies */
$header-color: $color-text !default
$header-hover-color: $color-accent !default

/* Couleurs de fond */
$header-background: transparent !default
```

L'animation du header (sticky) et des dropdowns est paramétrable :

```sass
$header-sticky-enabled: true !default

/* Couleurs */
$header-sticky-background: $color-background !default
$header-sticky-color: $header-color !default
$header-sticky-dropdown-background: $header-sticky-background !default

/* Transitions */
$header-transition: 0.3s !default
$header-sticky-transition: $header-transition !default
$header-dropdown-transition: $header-transition !default

/* Tailles et espacements */
$header-nav-padding-y: px2rem(30) !default
$header-logo-height: 32px !default
$header-logo-height-desktop: $header-logo-height !default
$header-height: 99px !default
$header-height-desktop: 74px !default
```

> L'option header-sticky-enabled détermine si la barre de navigation restera fixée ou non au haut de l'écran en scroll.

Customisation des sous-menus :

```sass
$header-dropdown-full: false !default

/* Couleurs */
$header-dropdown-background: $header-background !default
$header-dropdown-color: $header-color !default

/* Transition */
$header-dropdown-transition: $header-transition !default
```

> L'option header-dropdown-full change l'affichage des sous-menu et permet un affichage pleine largeur avec une mise en place des liens en colonnes.

Une variable permet de changer automatiquement la couleur du logo du site lorsque le header devient fixe :

```sass
$header-sticky-invert-logo: false !default
```

### Footer

```sass
/* Couleurs */
$footer-color: $color-text !default
$footer-background-color: $color-background-alt !default

/* Affichage du logo */
$footer-logo-height: $header-logo-height !default
$footer-logo-height-desktop: $footer-logo-height !default
```

### Hero

```sass
$hero-height: 300px !default
$hero-height-desktop: 400px !default
$hero-color: $color-text !default
$hero-background-color: $color-background-alt !default
```

### Icons

```sass
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

### Navigation

Définition du background de l'overlay qui apparaît lorsque les dropdowns du menu sont activés ou que le menu est développé en mobile :

```sass
$body-overlay-color: rgba(0, 0, 0, 0.3) !default
```

### Table of content

#### Couleurs

```sass
$toc-color: $color-text !default
$toc-active-color: $color-accent !default
$toc-background-color: $color-background-alt !default
```

#### Typographies

##### Liens simples

```sass
$toc-font-family: $body-font-family !default
$toc-font-size: $body-size !default
$toc-font-size-desktop: $body-size-desktop !default
$toc-line-height: $body-line-height !default
```

##### Titre du TOC

```sass
$toc-title-font-family: $meta-font-family !default
$toc-title-font-size: $meta-size !default
$toc-title-font-size-desktop: $meta-size-desktop !default
```

### Tableaux

Pour personnaliser l'apparence des typographies utilisées dans les tableaux de données :

```sass
$table-head-font-size: $h4-size !default
$table-head-font-size-desktop: $h4-size-desktop !default
$table-body-size: $body-size !default
$table-body-size-desktop: $body-size-desktop !default
```

## Blocks

### Call to action

```sass
$block-call-to-action-background: $color-accent !default
$block-call-to-action-color: $color-background !default

/* Apparence du bouton du premier lien */
$block-call-to-action-button-background: $color-background !default
$block-call-to-action-button-color: $color-text !default
```

### Definitions

```sass
/* Bordure inférieure de la définition */
$block-definition-border-color: $color-border !default
$block-definition-border-color-hovered: $color-accent !default

/* Typographie */
$block-definition-color-hovered: $color-accent !default
$block-definition-font-size: $body-size !default
$block-definition-font-size-desktop: $body-size-desktop !default
```

### Key figures

La taille de la police de ce bloc est personnalisable pour plusieurs breakpoints, pour les chiffres (`block-key_figures-number-font-size`) et leur légende (`block-key_figures-font-size`) :

```sass
$block-key_figures-font-size: px2rem(16) !default
$block-key_figures-number-font-size: px2rem(32) !default

$block-key_figures-font-size-desktop: px2rem(18) !default
$block-key_figures-number-font-size-desktop: px2rem(40) !default

$block-key_figures-font-size-lg: px2rem(20) !default
$block-key_figures-number-font-size-lg: px2rem(50) !default

$block-key_figures-font-size-xl: $block-key_figures-font-size-lg !default
$block-key_figures-number-font-size-xl: px2rem(60) !default

$block-key_figures-font-size-xxl: $block-key_figures-font-size-xl !default
$block-key_figures-number-font-size-xxl: px2rem(80) !default
```

### Gallery

La couleur de fond de la galerie est personnalisable :

```sass
$block-gallery-carousel-background: $color-background-alt
```

### Image

Pour personnaliser la largeur maximale d'une image, dans le cas des pages avec ou sans sidebar :

```sass
$block-image-max-height-with-sidebar: calc(100vh - var(--header-height)) !default
$block-image-max-height-without-sidebar: none !default
```

### Pages

Seul le layout cards est personnalisable :

```sass
/* Fond du bloc */
$block-pages-card-background: color-contrast($color-background, 10%) !default

/* Apparence des cartes */
$block-pages-card-page-background: invert($color-text) !default
$block-pages-card-page-background-hover: $color-accent !default
$block-pages-card-page-color: $color-text !default
$block-pages-card-page-color-hover: $color-background !default
```

### Testimonials

Paramètres par défaut :

```sass
/* Typographies */
$block-testimonials-color: $color-accent !default
$block-testimonials-font-family: $quote-font-family !default
$block-testimonials-font-size: $quote-size !default
$block-testimonials-line-height: $quote-line-height !default
$block-testimonials-style: $quote-style !default

/* Couleurs */
$block-testimonials-pagination-background: $color-border !default
$block-testimonials-pagination-progress-background: $color-accent !default
```

Pour les grands écrans :

```sass
$block-testimonials-xl-font-size: $quote-size-desktop-short !default
$block-testimonials-xl-line-height: $quote-line-height !default
$block-testimonials-xl-font-size-long-text: $quote-size-desktop-long !default
$block-testimonials-xl-line-height-long-text: $quote-line-height !default
```

### Timeline

```sass
$block-timeline-horizontal-background: $color-background-alt !default
$block-timeline-horizontal-color: $color-text !default
```

## Sections

```sass
$post-media-background: $article-media-background !default
$post-categories-color: color-contrast($color-text, 20%) !default
$post-time-color: $color-text-alt !default

/* Layout posts list (ne concerne pas les blocks posts) */
$posts-layout-list: true !default

/* Si layout posts grid (ne concerne pas les blocks posts) */
$posts-grid-columns: $block-posts-grid-columns !default
```

### Articles

```sass
$article-media-background: color-contrast($color-background, 3%) !default
$article-media-aspect-ratio: 2 !default
```

### Person

Personnalisation de la couleur de fond des ronds qui remplacent les photo d'une personne lorsqu'il n'y en a pas :

```sass
$persons-avatar-background-color: $color-background-alt !default
```

### Program

Font-size du cadre `.essential` :

```sass
$program-essential-font-size: $meta-size !default
$program-essential-font-size-desktop: $meta-size-desktop !default
```

Font-size du bouton de partage d'une formation :

```sass
$program-share-font-size: $meta-size !default
$program-share-font-size-desktop: $meta-size-desktop !default
```

Paramétrage du z-index de l'aside horizontal et sticky :

```sass
$program-zindex-toc: $zindex-toc !default
```

## MISC

### Animations

```sass
$arrow-ease-transition: cubic-bezier(0, 0.65, 0.4, 1.2) !default
$arrow-ease-transition-2: cubic-bezier(0, 0.65, 0.4, 1) !default
```