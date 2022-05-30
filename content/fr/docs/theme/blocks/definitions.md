---
title: "Liste de définitions"
description: >
  Bloc pour avoir une liste de définitions
---

## Présentation

![image](https://user-images.githubusercontent.com/7761386/170989374-8b73382d-50d2-4f52-918d-70f19e26c58c.jpg)


## Data

### JSON (Osuny)

* elements ```array```
  * title ```string```
  * text ```text```

### Static (Hugo)

```
- template: definitions
  title: >
    Définitions
  position: 1
  data:
    - title: >-
        Nom complet
      text: >-
        Tim Berners-Lee
    - title: >-
        Date de naissance
      text: >-
        8 juin 1955
    - title: >-
        Entreprises
      text: >-
        CERN (1984-1994)
        World Wide Web Consortium
        et bien d'autres...
```
