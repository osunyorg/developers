---
title: Pages
weight: 1
---

![image](https://raw.githubusercontent.com/osunyorg/admin/refs/heads/main/app/assets/images/communication/blocks/templates/pages.jpg)

```yaml {filename="Données Hugo"}
  - kind: block
    template: pages
    title: >-
      
    slug: >-
      
    ranks:
      self: 2
      children: 3
    data:
      options:
        image: true
        main_summary: true
        summary: true
      layout: grid
      pages:
        - permalink: "/actualites/"
          path: "/posts"
          slug: "actualites"
          file: "content/fr/posts/_index.html"
        - permalink: "/agenda/"
          path: "/events"
          slug: "agenda"
          file: "content/fr/events/_index.html"
        - permalink: "/contenus/"
          path: "/pages/contenus"
          slug: "contenus"
          file: "content/fr/pages/contenus/_index.html"
```


## Mise en page

### Grille

![image](https://raw.githubusercontent.com/osunyorg/admin/refs/heads/main/app/assets/images/communication/blocks/templates/pages/grid.png)
