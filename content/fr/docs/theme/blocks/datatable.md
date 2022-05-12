---
title: "Datatable"
description: >
  Bloc pour créer un tableau
---

## Présentation

Il y a un enjeu d’accessibilité sur ce sujet (caption, th, scope…). Bloc à travailler dans ce sens.

### Edit

* title ```text```
* headers ```array```
  * name ```string```
* rows ```array```
  * cells ```array```
* caption ```textarea```


### Static

```
- template: datatable
  title: >-
    Titre du bloc
  headers: 
    - name: Colonne 1
    - name: Colonne 2
    - name: Colonne 3
  rows:
    - ["valeur col 1", "valeur col 2", "valeur col 3"]
    - ["valeur col 1", "valeur col 2", "valeur col 3"]
    - ["valeur col 1", "valeur col 2", "valeur col 3"]
  caption: >-
    Texte qui décrit le contenu du tableau pour en faciliter l'accès (RGAA)
```
