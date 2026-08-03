---
title: Index
weight: 3
description: >
  Paramètres de chaque type de `section`
---

Vous pouvez configurer ces options pour chaque type de contenu (`posts`, `events`, `persons`, `organizations`...)

## Layouts

Chaque index permet d'overider la disposition des éléments issus d'une même nature éditoriale.

```yaml{filename="themes/osuny/hugo.yaml"}
pages:
  index:
    layout: grid
```

> Les layouts sont pour la plupart en `grid` par défaut.

Ces configuration permettent de paramétrer l'affichage de chaque élément, comme s'il s'agissait de bloc. Elles obéissent aux mêmes options :
```yaml{filename="themes/osuny/hugo.yaml"}
pages:
  index:
    ...
    options:
      categories: false
      image: true
      main_summary: false
      subtitle: true
      summary: true
```

En plus de ces options, tous les types d'objets permettent de tronquer ou non la description de chaque élément, via la clef `truncate_description`.

## Images par défaut

La clef `default_image` permet de paramétrer l'affichage ou non (par défaut, ce n'est pas le cas) de l'image par défaut paramétrée dans le site (dans la partie "réglages").

```yaml{filename="themes/osuny/hugo.yaml"}
jobs:
  default_image: false
```
