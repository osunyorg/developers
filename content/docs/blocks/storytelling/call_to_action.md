---
title: Appel à action
weight: 4
---

![image](https://raw.githubusercontent.com/osunyorg/admin/refs/heads/main/app/assets/images/communication/blocks/templates/call_to_action.jpg)

```yaml {filename="Données Hugo"}
  - kind: block
    template: call_to_action
    title: >-
      
    slug: >-
      
    ranks:
      self: 2
    data:
      layout: accent_background
      text: >-
        <p>Site de recettage</p>


      alt: >-
        

      credit: >-
        

      buttons:
        - title: >-
            Découvrir les projets

          url: >-
            https://recettage.demo.osuny.site/projets/

          target_blank: false
```

## Configuration style

```sass
// _theme/_default_config.sass
$block-call-to-action-background: $primary !default
$block-call-to-action-color: white !default
```
