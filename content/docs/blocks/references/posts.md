---
title: Actualités
weight: 2
---

![image](https://raw.githubusercontent.com/osunyorg/admin/refs/heads/main/app/assets/images/communication/blocks/templates/posts.jpg)

```yaml {filename="Données Hugo"}
  - kind: block
    template: posts
    title: >-
      Actualités
    slug: >-
      actualites
    ranks:
      self: 2
      children: 3
    data:
      mode: all
      all: true
      layout: grid
      options:
        author: false
        categories: false
        date: false
        image: true
        reading_time: false
        subtitle: true
        summary: true
      posts:
        - permalink: "/actualites/2024-05-17-lapplication-osuny-presente-le-niveau-de-securite-bon/"
          path: "/posts/2024/2024-05-17-lapplication-osuny-presente-le-niveau-de-securite-bon"
          slug: "lapplication-osuny-presente-le-niveau-de-securite-bon"
          file: "content/fr/posts/2024/2024-05-17-lapplication-osuny-presente-le-niveau-de-securite-bon.html"
        - permalink: "/actualites/2024-05-17-osuny-hal/"
          path: "/posts/2024/2024-05-17-osuny-hal"
          slug: "osuny-hal"
          file: "content/fr/posts/2024/2024-05-17-osuny-hal.html"
        - permalink: "/actualites/2024-05-17-lediteur-de-contenu-vient-de-sameliorer/"
          path: "/posts/2024/2024-05-17-lediteur-de-contenu-vient-de-sameliorer"
          slug: "lediteur-de-contenu-vient-de-sameliorer"
          file: "content/fr/posts/2024/2024-05-17-lediteur-de-contenu-vient-de-sameliorer.html"
```