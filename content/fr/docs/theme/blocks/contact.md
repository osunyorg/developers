---
title: "Contact"
description: >
  Bloc contact
---

## Présentation



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
}
```

### Static (Hugo)

* name ```string```
* phone_numbers ```array```
* emails ```array```
* address ```text```
* timetable ```hash```
  * title ```string```
  * time_slots ```hash```
    * from ```string```
    * to ```string```


```
- template: contact
    title: >-
      Contact
    position: 1
    data:
      name: >-
        my first contact

      address: >-
        10 rue de Bordeaux,
        
        33150 Bdx

      phone_numbers: ["dzedze", "sddd"]

      emails: ["hello@med"]

      timetable:
        - title: >-
            Lundi


          time_slots:
            - from: "09:00"
              to: "12:00"
            - from: "13:00"
              to: "16:00"

        - title: >-
            Mardi

          time_slots:
            - from: "09:00"
              to: "12:00"
            - from: "13:00"
              to: "16:00"

```
