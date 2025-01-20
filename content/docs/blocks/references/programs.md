---
title: Formations
weight: 6
---

![image](https://raw.githubusercontent.com/osunyorg/admin/refs/heads/main/app/assets/images/communication/blocks/templates/programs.jpg)

```yaml {filename="Données Hugo"}
  - kind: block
    template: programs
    title: >-
      
    slug: >-
      
    ranks:
      self: 2
      children: 3
    data:
      layout: list
      options:
        diploma: true
        image: false
        summary: false
      programs:
        - permalink: ""
          path: "/programs/infocom"
          slug: "infocom"
          file: "content/fr/programs/infocom/_index.html"
```