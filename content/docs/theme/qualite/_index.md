---
title: Assurance qualité
weight: 5
description: >
  Garantir la non régression
---

## Review de code (PR)

Comment relire une grosse PR ?

- Tester en local sur example
- Vérifier les breakpoints
- Vérifier les états (full-width / ou pas full-width)



## Percy
Percy est un outil de suivi créant des snapshots permettant des analyses visuelles.

1. Ajouter le token percy dans les secrets du repo (https://docs.percy.io/docs/github-actions)
2. Copier ou décommenter la [github action](https://github.com/noesya/osuny-hugo-template/blob/main/.github/workflows/percy.)
3. Préciser la liste des snapshot dans le fichier [.percy.yml](https://github.com/noesya/osuny-hugo-template/blob/main/.percy.yml)

