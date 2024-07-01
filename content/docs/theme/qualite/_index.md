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
[Percy](https://percy.io/) est un outil de suivi créant des snapshots permettant des analyses visuelles.

### Sur Percy
1. Cliquer sur « Create new project ».
2. Nommer le projet.
3. Dans les paramètres du projet (« Project settings »), changer `default base branch` et `auto-approve branches` par la branche modèle du site (`main` ou `master` en général).
4. Toujours dans les paramètres, récupérer le « project token ».

### Dans le github du site
Ajouter le token percy dans les secrets du repo (cf. [documentation de Percy](https://docs.percy.io/docs/github-actions)) (Settings > Action secrets > « New secret ») sous le nom `PERCY_TOKEN`.

### Dans le projet
1. Dans `.github/workflows/` ajouter un fichier `percy.yml` basé sur ce modèle : [template](https://github.com/noesya/osuny-hugo-template/blob/main/.github/workflows/percy.yml).
2. À la racine du projet, ajouter un fichier `percy.yml` basé sur ce modèle : [template](https://github.com/noesya/osuny-hugo-template/blob/main/.percy.yml)
3. À la racide du projet, configurez un `snapshots.yml` qui indexe toutes les pages que Percy examinera, de cette façon : 

``` yml {filename="snapshots.yml"}
serve: ./public
snapshots:
  - name: Home
    url: index.html
  - name: Titre d'une page
    url: ...
```

{{< callout type="info" >}}
  1. Chaque URL doit être précédée d'un `/` et suivie d'un `/index.html`
  2. Dans le cadre d'un site multilingue, bien ajouter `/fr/` avant l'URL.
{{< /callout >}}

### Utiliser Percy
Dans un premier temps, Percy va examiner les pages de la branche modèle. Pour qu'il compare cette branche avec une autre, il faut créer une pull request (**pas en draft**), le build de la comparaison se fera automatiquement à chaque push sur la branche.