---
title: "Organigramme"
description: >
  Bloc de témoignages en carousel
---
## Présentation

![image](https://user-images.githubusercontent.com/4457294/160695968-2d180031-fee0-4cfa-bc4a-d5ce438fb6bb.png)


## Edit

* elements ```each```
  * choix ```select```
  * rôle ```text```
* description ```text```

```json

{
  "elements":
    [
      {
      "id":"b5c39702-caa7-482d-8679-56ecf0f721c3", 
      "role": "Dev back"
      }, {
      "id":"ff731be6-6b2e-4e0a-8abc-823f3da577cf", 
      "role":"Dev front"
      }
    ], 
  "description": "Texte associé à l'organigramme"
}
```

## Static

```
- template: organization_chart
    title: >-
      Organigramme
    position: 1
    data:
      description: >-
        <p>Texte associé à l'organigramme</p>
        


      with_link: false

      with_photo: false

      persons:
        - slug: "alexis-benoit"
          role: >-
            Dev front


        - slug: "pierre-andre-boissinot"
          role: >-
            Dev back


        - slug: "sebastien-gaya"
          role: >-
            Dev back


        - slug: "arnaud-levy"
          role: >-
            Dev back


        - slug: "sebastien-moulene"
          role: >-
            Dev front
        
```