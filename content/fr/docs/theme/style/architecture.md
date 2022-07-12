---
title: "Architecture"
description: >
  Architecture des fichiers et dossiers SASS
---

# Réflexion sur la structure des fichiers de style

## Isomorphisme des arborescences html et sass

Essayer de rendre plus similaire l'organisation des fichiers de style et ceux de templating html.

### Arborescence des fichiers de style :

```
|-- assets/
    |____main.sass
    |_____theme
    | |____organizations
    | | |_____pages.sass
    | | |_____widgets.sass
    | |____researchers
    | | |_____pages.sass
    | | |_____widgets.sass
    | |____home
    | | |_____pages.sass
    | |____posts
    | | |_____pages.sass
    | | |_____widgets.sass
    | |____diplomas
    | | |_____pages.sass
    | | |_____widgets.sass
    | |____authors
    | | |_____pages.sass
    | | |_____widgets.sass
    | |____articles
    | | |_____pages.sass
    | | |_____widgets.sass
    | |____sitemap
    | | |_____pages.sass
    | |____teachers
    | | |_____pages.sass
    | | |_____widgets.sass
    | |_____default_config.sass
    | |____hugo-osuny.sass
    | |____programs
    | | |_____pages.sass
    | | |_____widgets.sass
    | |_____utils.sass
    | |____persons
    | | |_____pages.sass
    | | |_____widgets.sass
    | |____volumes
    | | |_____pages.sass
    | | |_____widgets.sass
    | |____pages
    | | |_____pages.sass
    | | |_____widgets.sass
    | |____categories
    | | |_____pages.sass
    | | |_____widgets.sass
    | |____partials
    | | |____blocks
    | | | |_____blocks.sass
    | | | |_____definitions.sass
    | | | |_____embed.sass
    | | | |_____chapter.sass
    | | | |_____gallery.sass
    | | | |_____organization_chart.sass
    | | | |_____partners.sass
    | | | |_____key_figures.sass
    | | | |_____timeline.sass
    | | | |_____testimonials.sass
    | | | |_____pages.sass
    | | | |_____image.sass
    | | | |_____files.sass
    | | | |_____contact.sass
    | | | |_____video.sass
    | | | |_____posts.sass
    | | | |_____datatable.sass
    | | | |_____call_to_action.sass
    | | |____footer
    | | | |_____footer-nav-expand.sass
    | | | |_____footer.sass
    | | | |_____gdpr.sass
    | | |____posts
    | | | |_____related.sass
    | | |____body
    | | | |_____content.sass
    | | | |_____grid.sass
    | | | |_____link.sass
    | | | |_____icons.sass
    | | | |_____typography.sass
    | | | |_____top.sass
    | | | |_____featured-image.sass
    | | | |_____global.sass
    | | | |_____main.sass
    | | |____a11y
    | | | |_____nav-accessibility.sass
    | | | |_____transcription.sass
    | | |____persons
    | | | |_____contacts-list.sass
    | | | |_____avatar.sass
    | | |____nav
    | | | |_____menu.sass
    | | | |_____menu-expand.sass
    | | | |_____sticky.sass
    | | | |_____toc.sass
    | | |____header
    | | | |_____hero.sass
    | | | |_____header.sass
    | | |____share
    | | | |_____share.sass
    | | | |_____dropdown-share.sass
    | |_____vendors
    | | |____bootstrap
    | | | |_____card.sass
    | | | |_____pagination.sass
    | | | |_____nav.sass
    | | | |_____dropdown.sass
    | | | |_____modal.sass
    | | | |_____row.sass
    | | | |_____breadcrumb.sass
    | | | |_____offcanvas.sass
    | | | |_____blockquote.sass
    | | | |_____btn.sass
    | | | |_____table.sass
    | | |____glightbox
    | | | |_____glightbox.sass
    | | |____splide
    | | | |_____splide.sass
    | |____administrators
    | | |_____pages.sass
    | | |_____widgets.sass
```

### L'organisation des fichiers de style :

1. Un dossier par **model / type** de contenu contenant les fichiers qui stylise le contenu en fonction du contexte :

    1. _pages.sass : les styles concernants les pages propres au modèle (index / show / term...)
    2. _widgets.sass : les styles concernants les partials relatif au model.
    
2. 

3. Un dossier **_vendors**, contenant les overrides des librairies utilisées (bootstrap / splide). Le restant des librairies est installé via npm.

4. Scoper le style du thème dans un dossier _theme pour faciliter la compréhension de l’override du thème et supprimer le dossier _custom. Cela permet de mieux ordonner le sass et de ne pas mélanger la création et l’ajout de nouveaux fichiers au site.

### Fichier de configuration du style du thème

Lors de la création d'un site, vous pouvez overridé le fichier configuration/_site.sass pour modifier les variables du thème par défaut, de bootstrap, et y déclarer toutes les variables nécessaires à l'intégration de votre site. Par exemple :

Dans **/assets/sass/_theme/_default/configuration/_site.sass

````
/*  Theme & Bootstrap variables
    Get all bootstrap variables in node_modules/bootstrap/scss/_variables.scss

$font-family-serif: 'Big Moore It', Serif
$text-width-8: calc((100%/12*8) - (24px/14)*5)
$text-width-10: calc((100%/12*10) - (24px/14)*3)

// Bootstrap
$primary: #FFFFFF
$body-bg: #222222
$body-color: #FFFFFF
$link-color: #FFFFFF

// Button
$btn-border-radius: 0

$gray-600: #CED4DA

$blockquote-font-size: px2rem(30)
$blockquote-footer-color: inherit
$blockquote-footer-font-size: 1rem
$blockquote-margin-y: 0

$border-color: rgba(white, .4)

$breadcrumb-active-color: white
$breadcrumb-font-size: px2rem(14)
$breadcrumb-margin-bottom: 0

$card-bg: transparent

$dropdown-bg: black
$dropdown-border-radius: 0
$dropdown-color: white
$dropdown-link-color: white
$dropdown-link-hover-bg: transparent

$font-family-sans-serif: 'Aestetico', Sans-Serif
$font-size-root: 1rem
$font-size-base: 1.125rem
$line-height-base: 1.6


$h1-font-size: px2rem(30)
$h2-font-size: px2rem(28)

$headings-font-weight: 300

$modal-inner-padding: 0

$navbar-padding-y: px2rem(25)

$navbar-brand-padding-y: 0
$navbar-brand-margin-end: 0

$small-font-size: px2rem(14)

$table-border-color: $border-color

$menu-background-color: black
$pagination-bg: transparent
$pagination-border-width: 0
$pagination-border-radius: 0
$pagination-border-color: transparent
$pagination-disabled-bg: transparent
$pagination-hover-bg: black
$pagination-active-bg: black
$pagination-focus-bg: black
````



### Organisation des fichiers js

Les fichiers js sont rangés dans deux sous-dossiers : **vendors** et **theme**. Le fichier **theme/body.js** est nécessaire, tandis que les **vendors** ou **cookie_consent** sont à ajouter si les fonctionnalités sont exploitées : cela permet de réduire les dépendances javascript et limiter le poids final du js compilé.

```javascript
    import './vendors/bootstrap';
    import './vendors/lightbox';
    import './vendors/carousel';
    import './theme/body';
    import './theme/cookie-banner';
```


