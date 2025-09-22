---
title: Publications
weight: 4
---

## Gestion des pages d'index

Hugo génère par défaut des pages d'index pour chaque section (type).

Dans le cas où on ne souhaite pas afficher la page d'index, il faut modifier ou ajouter un static d'index (`_index.html`) de la section avec ces paramètres de `build` :

```
---
build:
  list: always
  publishResources: true
  render: never
---
```

[Voir la documention hugo](https://gohugo.io/content-management/build-options/)

