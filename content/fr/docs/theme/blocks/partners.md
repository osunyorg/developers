---
title: "Partenaires"
description: >
  Bloc partenaires
---

## Présentation

![image](https://user-images.githubusercontent.com/4457294/160695991-7349a7ee-d4b1-4b34-b785-068cdbf2ed1d.png)


## Data

### JSON (Osuny)

* titre ```text```
* description  ```rich text```
* partenaire ```each```
  * choix ```select```
  * nom ```text```
  * website ```text```
  * logo ```file```

```json
{
  "elements": [
    {
      "id": "c83bab8c-a11c-479b-9f47-fb4dfea37486", 
      "name": "Noesya", 
      "url": "https://www.noesya.coop/", 
      "logo":{
        "id": "8e0b25d7-384d-49ae-830a-4f37a105036c", 
        "filename":"Logo-noesya.png", 
        "signed_id": "eyJfcmFpbHMiOnsibWVzc2..."
      }
    }
  ], 
  "description": ""
}
```

## Static

```
- template: partners
  title: >
    Partenaires
  position: 1
  data:
    description: >-
        


    partners:
      - name: >
          Noesya


        url: >-
          https://www.noesya.coop/


        logo: ""
```