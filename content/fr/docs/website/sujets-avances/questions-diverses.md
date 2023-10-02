---
title: "Questions diverses"
description: >
  De petits sujets qui peuvent être problématiques
---

## Ajouter le thème à un site Hugo existant

TODO traiter le passage en module Hugo ?

```bash
git submodule add https://github.com/noesya/osuny-hugo-theme-aaa.git themes/osuny-hugo-theme-aaa
yarn
```

Dans la configuration Hugo, définir le thème `osuny-hugo-theme-aaa`.

```bash
hugo server
```

## Mettre à jour tous les sites en local

Dans le dossier contenant tous les repos des sites : 

```bash
find . -type d -depth 1 -exec git --git-dir={}/.git --work-tree=$PWD/{} pull origin main --recurse-submodules \;
```

