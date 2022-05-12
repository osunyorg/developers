---
title: "Liste de pages"
description: >
  Bloc de pages sélectionnées
---

## Edit

* page principale ```page```
* option description_short ```toggle```
* pages ```each```
  * option image ```toggle```
  * option description_short ```toggle```


## Static

```
- template: pages
  title: >
    Team
  position: 11
  data:
    page: /equipe/
    show_descriptions: true
    show_images: false
    pages:
      - slug: /equipe/equipe-de-recherche/
      - slug: /equipe/equipe-pedagogique/
```

> La clé *slug* devra être remplacée par *page* 