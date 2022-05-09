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
    |-- main.sass
    |-- _theme/
        |-- hugo-osuny.sass
        |-- _default/
        |   |-- abstracts/
        |   |   |-- _functions.sass
        |   |   |-- _icons.sass
        |   |   |-- _mixins.sass
        |   |-- base/
        |   |   |-- _defaults.sass
        |   |   |-- _fonts-icons.sass
        |   |   |-- _fonts.sass
        |   |   |-- _typography.sass
        |   |-- configuration/
        |   |   |-- _site.sass              # Ce fichier de variables est destiné à être overridé 
        |   |   |-- _theme.sass             # Ce fichier ne doit pas être overridé car il contient les variables du thème nécessaires à son fonctionnement. Ce fichier importe "_site.sass" ce qui permet un override et un ordre d'appel fonctionnel.
        |   |-- layouts/
        |   |   |-- _footer.sass
        |   |   |-- _header.sass
        |   |   |-- _main.sass
        |   |   …
        |   |-- partials/
        |       |-- _avatar.sass
        |       |-- _contacts-list.sass
        |       |-- _featured-image.sass
        |       |-- _footer-nav-expand.sass
        |       |-- _hero.sass
        |       …
        |-- _vendors
        |   |-- bootstrap
        |   |   |-- _blockquote.sass
        |   |   |-- _breadcrumb.sass
        |   |   |-- _btn.sass
        |   |   …
        |   |-- splide
        |       |-- _splide.sass
        |   |-- glightbox
        |       |-- _glightbox.sass
        |-- administrators
        |   |-- _pages.sass
        |   |-- _widgets.sass
        |-- articles
        |   |-- _pages.sass
        |   |-- _widgets.sass
        |-- authors
        |   |-- _pages.sass
        |   |-- _widgets.sass
        |-- blocks                          # Nous avons fait le choix de placer les blocks au même niveau que les modèles car ceci contient des informations proopre
        |   |-- _blocks.sass
        |   |-- _call_to_action.sass
        |   |-- _chapter.sass
        |   |-- _gallery.sass
        |   …
        |-- categories
        |   |-- _pages.sass
        |   |-- _widgets.sass
        |-- home
        |   |-- _pages.sass
        |-- organizations
        |   |-- _pages.sass
        |   |-- _widgets.sass
        |-- pages
        |   |-- _pages.sass
        |   |-- _widgets.sass
        |-- persons
        |   |-- _pages.sass
        |   |-- _widgets.sass
        |-- posts
        |   |-- _pages.sass
        |   |-- _widgets.sass
        |-- programs
        |   |-- _pages.sass
        |   |-- _widgets.sass
        |-- researchers
        |   |-- _pages.sass
        |   |-- _widgets.sass
        |-- sitemap
        |   |-- _pages.sass
        |-- teachers
        |   |-- _pages.sass
        |   |-- _widgets.sass
        |-- volumes
            |-- _pages.sass
            |-- _widgets.sass
```

### L'organisation des fichiers de style :

1. Un dossier par **model / type** de contenu contenant les fichiers qui stylise le contenu en fonction du contexte :

    1. _pages.sass : les styles concernants les pages propres au modèle (index / show / term...)
    2. _widgets.sass : les styles concernants les partials relatif au model.
    
2. Un dossier **_default**, contenant tout le style propre aux éléments de mise en page qui ne possède pas de contenu/type propre. À l’intérieur, une découpe par fichier :
    1. *abstracts* : tout le style qui ne générera pas de css (variables, functions, mixins...)
    2. *base* : tout ce qui concerne la base du site (le design system typographic, les familles de typo, les icons, les states généraux — ::selection, ::hover...)
    3. *blocks* : le style propre aux blocks qui composent les pages —> **à déplacer à la racine de _theme ?** Certains blocks contiennent du contenu propre mais d’autres servent à de la mise en page 
    4. *layouts* : le style concernant les contenants uniquement, ils servent généralement à assembler des components/partials 
    5. *partials*: le style concernant les components ou petits éléments génériques (liens, icônes, breadcrumbs...)
    6. un fichier *_configurations* : il faut ici mettre les configurations pour facilement overrider un thème sans entrer dans tous les fichiers
3. Un dossier **_vendors**, contenant les overrides des librairies utilisées (bootstrap / splide). Le restant des librairies est installé via npm.


4. Scoper le style du thème dans un dossier _theme pour faciliter la compréhension de l’override du thème et supprimer le dossier _custom. Cela permet de mieux ordonner le sass et de ne pas mélanger la création et l’ajout de nouveaux fichiers au site.

### Fichier de configuration du style du thème

L'arborescence du thème se présente de cette manière : 


### Organisation des fichiers js

Les fichiers js sont rangés dans deux sous-dossiers : **vendors** et **theme**. Le fichier **theme/body.js** est nécessaire, tandis que les **vendors** ou **cookie_consent** sont à ajouter si les fonctionnalités sont exploitées : cela permet de réduire les dépendances javascript et limiter le poids final du js compilé.

```javascript
    import './vendors/bootstrap';
    import './vendors/lightbox';
    import './vendors/carousel';
    import './theme/body';
    import './theme/cookie-banner';
```


