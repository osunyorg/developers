---
title: Personnes
weight: 3
---

![image](https://raw.githubusercontent.com/osunyorg/admin/refs/heads/main/app/assets/images/communication/blocks/templates/persons.jpg)

```yaml {filename="Données Hugo"}

  - kind: block
    template: persons
    title: >-
      
    slug: >-
      
    ranks:
      self: 2
      children: 3
    data:
      description: >-
        
      options:
        image: true
        summary: true
        link: true
      persons:
        - permalink: "/equipe/arnaud-levy/"
          path: "/persons/arnaud-levy"
          slug: "arnaud-levy"
          file: "content/fr/persons/arnaud-levy.html"
          role: >-
            Maître de conférences associé
```