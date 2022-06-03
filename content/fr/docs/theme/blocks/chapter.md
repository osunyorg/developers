---
title: "Chapitre"
description: >
  Bloc de texte avec notes et image
---

## Présentation

![image](https://user-images.githubusercontent.com/4457294/160695826-f30b32bf-3434-4bd6-9f1e-ba42de91fec1.png)


## Data

### JSON (Osuny)

* text ```richtext (mini-list)```
* notes ```richtext (mini)```
* image ```blob```
* image_alt ```string```
* image_credit ```string```

```json
{
  "text": "<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed id felis et nunc euismod consectetur. Maecenas eu tortor nunc. Duis aliquet mauris mauris, in fermentum ex tempus sed.</p><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed id felis et nunc euismod consectetur. Maecenas eu tortor nunc. Duis aliquet mauris mauris, in fermentum ex tempus sed.<br></p>",
  "notes": "<p>Notes de chapitre</p>",
  "image": {
    "id": "290c9549-73a7-412c-b902-92403f486861",
    "filename": "image.jpeg",
    "signed_id": "eyJfcmFpbHMiOnsibWVz..."
  },
  "image_alt": "texte alternatif de l'image",
  "image_credit": "crédit de l'image"
}
```

### Static (Hugo)

* text ```richtext```
* notes ```richtext```
* image ```image```

```
- template: chapter
  title: >-
    Chapitre 1
  position: 1
  data:
    text: >-
      <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed id felis et nunc euismod consectetur. Maecenas eu tortor nunc. Duis aliquet mauris mauris, in fermentum ex tempus sed.</p><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed id felis et nunc euismod consectetur. Maecenas eu tortor nunc. Duis aliquet mauris mauris, in fermentum ex tempus sed.<br></p>
    notes: >-
      <p>Notes de chapitre</p>
    image:
      id: "290c9549-73a7-412c-b902-92403f486861"
      alt: >-
        Texte alternatif de l'image
      credit: >-
        Crédit de l'image
```
