---
title: "Image"
description: >
  Bloc pour afficher une image
---

## Présentation

![image](https://user-images.githubusercontent.com/7761386/171760808-3df11155-cf8b-4905-92d6-84e064fa6c87.jpg)


## Data

### JSON (Osuny)

* image ```blob```
  * alt ```string```
  * credit ```string```
* text ```richtext (mini)```

```json
{
  "image": {
    "id": "453a131c-fded-4c9f-b647-0ca1871736f1",
    "filename": "picture.jpeg",
    "signed_id": "eyJfcmFpbHMiOnsibWVz..."
  },
  "alt": "Un paysage verdoyant",
  "credit": "Photo par Jean Dupont",
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
        Image d'exemple


      image:
        id: "1587c1df-f29b-451d-bddc-3295a91cf13c"
        file: "dfd1162d-e3d0-43b0-a7ff-6d3c5749dc2b"


        alt: >-
          Un paysage verdoyant


        credit: >-
          <p>Photo par Jean Dupont</p>
```
