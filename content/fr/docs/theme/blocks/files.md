---
title: "Fichiers"
description: >
  Bloc pour lister des fichiers à télécharger
---

## Présentation

![image](https://user-images.githubusercontent.com/7761386/171012791-dad5c921-241c-4275-8966-981c2032d636.jpg)


## Data

### JSON (Osuny)

* elements ```array```
  * title ```string```
  * file ```blob```

```json
{
  "elements": [
    {
      "title": "Course de 5km",
      "file": {
        "id": "9f2fde46-9de0-4b02-97e1-0500fc0bd2da",
        "filename": "5k.png",
        "signed_id": "eyJfcmFpbHMiOnsibWVz..."
      }
    },
    {
      "title": "Course de 10km",
      "file": {
        "id": "af05c47b-e3e2-473d-90b1-3593d53dc3a2",
        "filename": "10k.png",
        "signed_id": "eyJfcmFpbHMiOnsibWVz..."
      }
    }
  ]
}
```

## Static (Hugo)

* files ```array```
  * title ```string```
  * file ```references (ActiveStorage::Blob)```

```
- template: files
  title: >-
    Titre du bloc
  position: 1
  data:
    files:
      - title: >-
          Course de 5km
        file: "9f2fde46-9de0-4b02-97e1-0500fc0bd2da"
      - title: >-
          Course de 10km
        file: "af05c47b-e3e2-473d-90b1-3593d53dc3a2"
```
