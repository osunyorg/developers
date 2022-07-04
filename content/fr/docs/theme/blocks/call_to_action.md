---
title: "Appel à action"
description: >
  Bloc call to action
---

## Présentation

![image](https://user-images.githubusercontent.com/7761386/170986725-a100f286-1f00-4ad0-9f7e-01bc2a47a48c.jpg)

## Data

### JSON (Osuny)

* text ```richtext (mini)```
* image ```blob```
  * alt ```string```
  * credit ```richtext```
* elements
  * title ```string```
  * url ```string```
  * target_blank ```boolean```

```json
{
  "elements": [
    {
      "title": "Osuny", 
      "url": "https://www.osuny.org", 
      "target_blank": false
    }, {
      "title": "Noesya", 
      "url": "https://www.noesya.coop", 
      "target_blank": true
      }, {
        "title": "Hugo", 
        "url": "https://gohugo.io", 
        "target_blank" : false
    },
  ], 
  "text": "<p>Lorem ipsum</p>", 
  "image": {
    "id": "2a8c2241-04c5-4ff3-aa0b-a1240155f1f4", 
    "filename": "ios-14-blue-and-pink-abstract-fnkg9z594gq55bfl.jpg", 
    "signed_id" :"eyJfcmFpbHMiOnsibWVzc2FnZSI6IkJBaEpJaWt5WVRoak1..."
    }, 
    "alt": "Texte alternatif de l'image", 
    "credit": "<p>Crédit de l'image</p>",
}
```

### Static (Hugo)

* text ```richtext```
* image ```image```
* buttons ```hash```
  * title ```string```
  * text ```string```
  * url ```string```
  * target_blank ```boolean```

```
- template: call_to_action
    title: >-
      CTA
    position: 1
    data:
      text: >-
        <p>Lorem ipsum</p>


      image:
        id: "7ce9ae91-3c9a-4700-ac2d-157df2e88855"
        file: "2a8c2241-04c5-4ff3-aa0b-a1240155f1f4"


      alt: >-
        Texte alternatif de l'image


      credit: >-
        <p>Crédit de l'image</p>


      buttons:
        - title: >-
            Osuny


          url: >-
            https://www.osuny.org


          target_blank: false


        - title: >-
            Noesya


          url: >-
            https://www.noesya.coop


          target_blank: true


        - title: >-
            Hugo


          url: >-
            https://gohugo.io


          target_blank: true
```
