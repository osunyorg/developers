---
title: Publications
weight: 10
description: >
  Option pour modifier le fonctionnement des publications
---


## Filtrer par projet de recherche (ANR)

Vous pouvez filtrer les publications par code ANR en modifiant le fichier config.yaml de votre site. Seules les publications ayant un code ANR correspondants à un ou plusieurs des codes ANR fournis en configuration seront affichés dans les listes des personnes et sur l'index des publications.

Le filtre étant en aval des données fournies par Osuny, les autres publications des chercheuses et chercheurs seront également inclues dans le site mais simplement absentes des listes.

### Exemple 

Dans config.yaml :

```
publications:
    filters:
      anr:
        - ANR-23-IACL-0001
        - ANR-10-IAHU-0004
        - ANR-19-P3IA-0002
```
