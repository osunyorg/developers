---
title: Mettre à jour le thème
weight: 5
---

## Mettre à jour le thème
Pour récupérer la dernière version du thème :
```bash
git pull --recurse-submodules
```

## Mettre à jour le template
Pour faire la mise à jour du template :
```bash
git remote add template git@github.com:noesya/osuny-hugo-template-aaa.git
git fetch --all
git merge template/main --allow-unrelated-histories
```

Malheureusement, cette méthode génère des conflits, qu'il faut traiter à la main.
