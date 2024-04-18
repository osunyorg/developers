---
title: Menus
weight: 2
---

## Menu

Attributes:
- university:references
- website:references
- title:string
- identifier:string

https://github.com/osunyorg/admin/blob/main/app/models/communication/website/menu.rb

## Item

Attributes:
- university:references
- website:references
- menu:references
- title:string
- parent:references
- position:integer
- kind:integer (enum: page, url)
- about:references (polymorphic)

https://github.com/osunyorg/admin/blob/main/app/models/communication/website/menu/item.rb

## Export

```yaml{filename="_data/menus.yml"}
primary:
    - title: Accueil
      target: /
    - title: Formations
      target: /formations
      children:
          - title: DUT
            target: /formations/dut
    - title: ENT
      target: https://ent.u-bordeaux3.fr
legal:
    - title: Mentions légales
      target: /mentions-legales
```

## Menu automatique

