---
title: "Timeline"
description: >
  Bloc pour afficher une timeline
---

## Présentation

![image](https://user-images.githubusercontent.com/7761386/171764607-94ed6bb6-8ff8-4a91-8bd7-3aa1c4577853.jpg)


## Data

### JSON (Osuny)

* elements ```array```
  * title ```string```
  * text ```text```

```json
{
  "elements": [
    {
      "title": "Décembre 2019",
      "text": "COP 25 (Madrid)"
    },
    {
      "title": "Novembre 2021",
      "text": "COP 26 (Glasgow)"
    },
    {
      "title": "Novembre 2022",
      "text": "COP 27 (Charm el-Cheikh)"
    }
  ]
}
```

### Static (Hugo)

* events ```array```
  * title ```string```
  * text ```text```

```
- template: timeline
  title: >-
    Timeline
  position: 1
  data:
    events:
      - title: >-
          Décembre 2019
        text: >-
          COP 25 (Madrid)
      - title: >-
          Novembre 2021
        text: >-
          COP 26 (Glasgow)
      - title: >-
          Novembre 2022
        text: >-
          COP 27 (Charm el-Cheikh)
```
