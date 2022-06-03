---
title: "Image"
description: >
  Bloc pour afficher une image
---

## Présentation

Image à insérer


## Data

### JSON (Osuny)

* image ```blob```
* image_alt ```string```
* image_credit ```string```
* text ```richtext (mini)```

```json
{
  "image": {
    "id": "453a131c-fded-4c9f-b647-0ca1871736f1",
    "filename": "picture.jpeg",
    "signed_id": "eyJfcmFpbHMiOnsibWVz..."
  },
  "image_alt": "Un paysage verdoyant",
  "image_credit": "Photo par Jean Dupont",
  "text": "<p>Image d'exemple</p>"
}
```

## Static (Hugo)

* text ```richtext```
* image ```image```

```
- template: image
  title: >-
    Titre du bloc
  position: 1
  data:
    text: >-
      <p>Image d'exemple</p>
    image:
      id: "1587c1df-f29b-451d-bddc-3295a91cf13c"
      alt: >-
        Un paysage verdoyant
      credit: >-
        Photo par Jean Dupont
```
