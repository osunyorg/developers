---
title: Tableau de données
weight: 6
---

![image](https://raw.githubusercontent.com/osunyorg/admin/refs/heads/main/app/assets/images/communication/blocks/templates/datatable.jpg)

```yaml {filename="Données Hugo"}
  - kind: block
    template: datatable
    title: >-
      
    slug: >-
      
    ranks:
      self: 2
    data:
      description: >-
        

      columns: ["Colonne 1", "Colonne 2"]

      rows:
        - ["A", "B"]

      caption: >-
        
```

## Accessibilité

Il y a un enjeu d’accessibilité sur ce sujet (caption, th, scope…). Bloc à travailler dans ce sens.

https://www.a11yproject.com/posts/accessible-data-tables/