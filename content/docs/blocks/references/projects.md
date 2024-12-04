---
title: Projets
weight: 8
---

![image](https://raw.githubusercontent.com/osunyorg/admin/refs/heads/main/app/assets/images/communication/blocks/templates/projects.jpg)

```yaml {filename="Données Hugo"}
  - kind: block
    template: projects
    title: >-
      
    slug: >-
      
    ranks:
      self: 2
      children: 3
    data:
      mode: all
      all: true
      layout: list
      options:
        categories: true
        image: true
        subtitle: true
        summary: false
        year: true
      projects:
        - permalink: "/projets/2023-bonnes-notes/"
          path: "/projects/2023-bonnes-notes"
          slug: "bonnes-notes"
          file: "content/fr/projects/2023-bonnes-notes.html"
        - permalink: "/projets/2023-nig/"
          path: "/projects/2023-nig"
          slug: "nig"
          file: "content/fr/projects/2023-nig.html"
        - permalink: "/projets/2023-ran-coper/"
          path: "/projects/2023-ran-coper"
          slug: "ran-coper"
          file: "content/fr/projects/2023-ran-coper.html"
```