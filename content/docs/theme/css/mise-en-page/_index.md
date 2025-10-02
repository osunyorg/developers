---
title: Mise en page
weight: 3
---

## Principes

Les blocs ont différentes mises en page que l'on appelle `layout`. Certains sont standards, d'autres sont spécifiques.

### Blocs de liste

Il existe des `layouts` standards pour les blocs de listes :

- alternate
- carousel
- cards
- grid
- large
- list

Pour ces blocs, on utilise des mixins pour factoriser les comportements.

```sass
@mixin layout-alternate
@mixin layout-carousel
@mixin layout-cards
@mixin layout-grid
@mixin layout-large
@mixin layout-list
```

Un mixin permet d'appliquer une sélection de `layouts` par défaut :

```sass
@mixin layout-defaults($selector)
    ul#{$selector}
        @include list-reset
    #{$selector}
        &--alternate
            @include layout-alternate
        &--grid
            @include layout-grid
        &--large
            @include layout-large
        &--list
            @include layout-list
```

#### Exemple :

Dans `/sass/_theme/sections/jobs.sass` :

```
@include layout-defaults(".jobs")
```

<!-- 
### Blocs spécifiques

D'autres `layouts` sont réservés à un ou plusieurs blocs.

TODO : lister les blocs et leur mise en page dans une page dédiée
#### Bloc chapitre : 

- simple (sans mise en avant)
- mise en avant légère
- mise en avant forte

#### Bloc appel à action : 

- mise en avant légère
- mise en avant forte
###  -->
