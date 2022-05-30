---
title: "Appel à action"
description: >
  Bloc call to action
---

## Présentation

![image](https://user-images.githubusercontent.com/7761386/170986725-a100f286-1f00-4ad0-9f7e-01bc2a47a48c.jpg)

## Data

### JSON (Osuny)

* text ```richtext (mini)```
* image ```file```
* image_alt ```string```
* image_credit ```string```
* button ```string```
* url ```string```
* button_secondary ```string```
* url_secondary ```string```
* button_tertiary ```string```
* url_tertiary ```string```


### Static (Hugo)

```
- template: call_to_action
  title: >
    CTA
  position: 1
  data:
    text: >-
      <p>Lorem ipsum</p>
    image:
      id: "7ce9ae91-3c9a-4700-ac2d-157df2e88855"
      alt: >-
        Texte alternatif de l'image
      credit: >-
        Crédit de l'image
    button:
      text: >-
        Osuny
      url: >-
        https://www.osuny.org
    button_secondary:
      text: >-
        Noesya
      url: >-
        https://www.noesya.coop
    button_tertiary:
      text: >-
        Hugo
      url: >-
        https://gohugo.io
```
