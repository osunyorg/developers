---
title: "Galerie"
description: >
  Bloc galerie d’images
---

## Présentation

![image](https://user-images.githubusercontent.com/4457294/160696042-2ef6aa5d-3135-4c60-ab8b-c373743220cf.png)


## Data

### JSON (Osuny)

* layout ```enum (grid, carousel)```
* elements ```array```
  * file ```blob```
  * alt ```string```
  * credit ```string```
  * text ```text```

```json
{
  "layout": "grid",
  "elements": [
    {
      "file": {
        "id": "d90208f3-f694-419d-983e-17965aa0484d",
        "filename": "alisa-anton-JhxGkGgd3Sw-unsplash-1920.jpg",
        "signed_id": "eyJfcmFpbHMiOnsibWVz..."
      },
      "alt": "Texte alternatif de la photo",
      "credit": "Photo par Alisa Anton",
      "text": "Petit café, petit livre"
    },
    {
      "file": {
        "id": "dc9cde16-4f76-45ed-85fa-3a61d0b1e356",
        "filename": "joseph-ashraf-pNWa4OOIEa8-unsplash-1920.jpg",
        "signed_id": "eyJfcmFpbHMiOnsibWVz..."
      },
      "alt": "Texte alternatif de la photo",
      "credit": "Photo par Joseph Ashraf",
      "text": "Pyramides en Egypte"
    },
    {
      "file": {
        "id": "2dc69784-f6e7-46a0-a3f8-80800374c4d4",
        "filename": "verstappen-photography-HSzG00PhyPc-unsplash-1920.jpg",
        "signed_id": "eyJfcmFpbHMiOnsibWVz..."
      },
      "alt": "Texte alternatif de la photo",
      "credit": "Photo par Verstappen",
      "text": "Tour au coucher de soleil"
    }
  ]
}
```

### Static (Hugo)

* images ```array```
  * ```image```
  * text ```text```

```
- template: gallery
  title: >-
    Galerie
  position: 1
  data:
    images:
      - id: "d90208f3-f694-419d-983e-17965aa0484d"
        alt: >-
          Texte alternatif de la photo
        credit: >-
          Photo par Alisa Anton
        text: >-
          Petit café, petit livre
      - id: "dc9cde16-4f76-45ed-85fa-3a61d0b1e356"
        alt: >-
          Texte alternatif de la photo
        credit: >-
          Photo par Joseph Ashraf
        text: >-
          Pyramides en Egypte
      - id: "2dc69784-f6e7-46a0-a3f8-80800374c4d4"
        alt: >-
          Texte alternatif de la photo
        credit: >-
          Photo par Verstappen
        text: >-
          Tour au coucher de soleil
```