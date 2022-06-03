---
title: "Chiffre clés"
description: >
  Bloc pour avoir une liste de chiffres clés
---

## Présentation

Image à insérer


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
  position: 1
  data:
      figures:
        - number: >-
            13000
          unit: >-
            m²
          description: >-
            au centre-ville
        - number: >-
            1000
          unit: >-

          description: >-
            étudiant·e·s
        - number: >-
            300
          unit: >-

          description: >-
            enseignant·e·s

```
