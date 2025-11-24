---
title: Term (catégorie)
weight: 2
description: >
  Option pour modifier le fonctionnement ou l'apparence d'une page de catégorie
---

Vous pouvez configurer ces options pour chaque type de contenu (`posts`, `events`, `persons`, `organizations`...). Ou de façon global sur la clé `params.categories`.

## Les sélecteurs de catégories

Les pages d'index ont des sélecteurs de catégories triées par taxonomies.

![Sélecteurs sur l'index](index-with-categories-dropdown.png)

Il est possible de conserver ces sélecteurs sur les pages des catégories.

![Sélecteurs dans la page d'une catégorie](term-categories-dropdown.png)

#### Options par défaut

Vous pouvez modifier les options par défaut pour modifier l'affichage de tous les types de contenu :

```yml
params:
  categories:
    single:
      taxonomies:
        display: true
```

Par exemple pour afficher ou masquer les taxonomies sur la single d'une `person`.

#### Masqué :

```yml
params:
  persons:
    categories:
      taxonomies:
        display: true
```
