---
title: "Témoignages"
description: >
  Bloc de témoignages en carousel
---

## Présentation

![image](https://user-images.githubusercontent.com/4457294/160696175-820e9ed0-3ce7-4a9b-bdca-44fb432812f9.png)


## Data

### JSON (Osuny)

* elements ```array```
  * text ```text```
  * author ```string```
  * job ```string```
  * photo ```blob```

```json
{
  "elements": [
    {
      "text": "Suspendisse non posuere nibh, eu molestie libero. Sed interdum erat id orci dictum, a vehicula justo rutrum.",
      "author": "John Doe",
      "job": "Auteur",
      "photo": {
        "id": "133bfa15-a02b-40d9-9eea-705162672ce2",
        "filename": "john-doe.jpeg",
        "signed_id": "eyJfcmFpbHMiOnsibWVz..."
      }
    },
    {
      "text": "Proin leo neque, imperdiet id tristique in, porttitor eu sem. Proin bibendum justo ultricies, imperdiet arcu id, blandit dui.",
      "author": "Jean Dupont",
      "job": "Éditeur",
      "photo": {
        "id": "6b2940b2-c166-4836-a430-a4d1f08818f9",
        "filename": "jean-dupont.jpeg",
        "signed_id": "eyJfcmFpbHMiOnsibWVz..."
      }
    }
  ]
}
```

## Static (Hugo)

* testimonials ```array```
  * text ```text```
  * author ```string```
  * job ```string```
  * photo ```references (ActiveStorage::Blob)```

```
- template: testimonials
  title: >-
    Témoignages
  position: 1
  data:
    testimonials:
      - text: >-
          Suspendisse non posuere nibh, eu molestie libero. Sed interdum erat id orci dictum, a vehicula justo rutrum.
        author: >-
            John Doe
        job: >-
            Auteur
        photo: "133bfa15-a02b-40d9-9eea-705162672ce2"
      - text: >-
          Proin leo neque, imperdiet id tristique in, porttitor eu sem. Proin bibendum justo ultricies, imperdiet arcu id, blandit dui.
        author: >-
            Jean Dupont
        job: >-
            Éditeur
        photo: "6b2940b2-c166-4836-a430-a4d1f08818f9"
```
