---
title: Campus
weight: 7
---

![image](https://raw.githubusercontent.com/osunyorg/admin/refs/heads/main/app/assets/images/communication/blocks/templates/locations.jpg)

```yaml {filename="Données Hugo"}
  - kind: block
    template: locations
    title: >-
      
    slug: >-
      
    ranks:
      self: 2
      children: 3
    data:
      layout: map
      options:
        image: true
        summary: true
      locations:
        - permalink: ""
          path: "/locations/noesya-bordeaux"
          slug: "noesya-bordeaux"
          file: "content/fr/locations/noesya-bordeaux/_index.html"
```