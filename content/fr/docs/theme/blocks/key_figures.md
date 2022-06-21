---
title: "Chiffre clés"
description: >
  Bloc pour avoir une liste de chiffres clés
---

## Présentation

![image](https://user-images.githubusercontent.com/7761386/171762150-456a6a1b-1268-4764-9371-0a29d10c92f2.jpg)


## Data

### JSON (Osuny)

* elements ```array```
  * number ```integer```
  * unit ```string```
  * description ```string```

```json
{
  "elements": [
    {
      "number": 13000,
      "unit": "m²",
      "description": "au centre-ville"
    },
    {
      "number": 1000,
      "unit": "",
      "description": "étudiant·e·s"
    },
    {
      "number": 300,
      "unit": "",
      "description": "enseignant·e·s"
    }
  ]
}
```

## Static (Hugo)

* figures ```array```
  * number ```string```
  * unit ```string```
  * description ```string```

```
- template: key_figures
    title: >-
      Titre du bloc
    position: 9
    data:
      figures:
        - number: 13000

          unit: >-
            m²


          description: >-
            au centre-ville


        - number: 1000

          unit: >-
            


          description: >-
            étudiant·e·s


        - number: 300

          unit: >-
            


          description: >-
            enseignant·e·s
```
