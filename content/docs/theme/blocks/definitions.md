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
  * description ```text```

```json
{
  "elements": [
    {
      "title": "Nom complet",
      "description": "Tim Berners-Lee"
    },
    {
      "title": "Date de naissance",
      "description": "8 juin 1955"
    },
    {
      "title": "Entreprises",
      "description": "CERN (1984-1994)\nWorld Wide Web Consortium\net bien d'autres..."
    }
  ]
}
```

### Static (Hugo)

* elements ```array```
  * title ```string```
  * description ```text```

```
 - template: definitions
    title: >-
      Un bloc définitions
    position: 1
    data:
      elements:
        - title: >-
            Nom complet


          description: >-
            Tim Berners-Lee


        - title: >-
            Date de naissance


          description: >-
            8 juin 1955


        - title: >-
            Entreprises


          description: >-
            CERN (1984-1994)
            World Wide Web Consortium
            et bien d'autres...
```
