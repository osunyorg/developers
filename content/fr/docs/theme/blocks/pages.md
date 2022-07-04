---
title: "Liste de pages"
description: >
  Bloc de pages sélectionnées
---

## Edit

* page principale ```page```
* option image ```toggle```
* option description_short ```toggle```
* pages ```each```

```json
{"elements":[
  {"id": "b679faae-59dd-4669-9847-3ae6e7943e9c"}, 
  {"id": "71be9b18-b350-48e1-9e0b-8f5a7bf33951"}, 
  {"id": "3efc6ae9-4e2c-453d-b30f-b66baf1a5e3b"}
  ], 
  "layout": "cards", 
  "mode": "selection", 
  "text":"", 
  "page_id": "b12b020e-e7ad-4b3a-a903-1c73babcd1ad", 
  "show_main_description": false, 
  "show_description": false, 
  "show_image": false
}
```

## Static

```
- template: pages
  title: >
    Team
  position: 1
  data:
    page: /equipe/
    show_descriptions: true
    show_images: false
    pages:
      - slug: /equipe/equipe-de-recherche/
      - slug: /equipe/equipe-pedagogique/
```

> La clé *slug* devra être remplacée par *page* 


## Configuration style

```(sass)
// _theme/_default_config.sass
$block-pages-card-background: #f8f9fa !default
```