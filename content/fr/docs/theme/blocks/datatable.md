---
title: "Datatable"
description: >
  Bloc pour créer un tableau
---

## Présentation

Il y a un enjeu d’accessibilité sur ce sujet (caption, th, scope…). Bloc à travailler dans ce sens.

## Data

### JSON (Osuny)

* elements ```text```
  * rows ```array```
    * cells ```array```
  * columns ```array```
  * caption ```textarea```

```json 
{
  "elements": [
    {
      "cells": ["valeur col 1", "valeur col 2", "valeur col 3"]
    }, {
      "cells": ["valeur col 1", "valeur col 2", "valeur col 3"]
    }], 
    "columns": ["Colonne 1", "Colonne 2"], 
    "caption": "Texte qui décrit le contenu du tableau pour en faciliter l'accès (RGAA)."
}
```

### Static

* columns ```array```
* rows ```array```
* caption ```string```
  
```
- template: datatable
    title: >-
      Titre du bloc
    position: 1
    data:

      columns: ["Colonne 1", "Colonne 2"]


      rows:

        - ["valeur col 1", "valeur col 2", "valeur col 3"]



        - ["valeur col 1", "valeur col 2", "valeur col 3"]


      caption: >-
        Texte qui décrit le contenu du tableau pour en faciliter l'accès (RGAA).
    
```
