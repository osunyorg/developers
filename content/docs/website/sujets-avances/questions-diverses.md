---
title: "Questions diverses"
description: >
  De petits sujets qui peuvent être problématiques
---

## Ajouter le thème à un site Hugo existant

TODO traiter le passage en module Hugo ?

```bash
git submodule add https://github.com/osunyorg/theme.git themes/osuny-hugo-theme-aaa
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


# Déploiement d'une modification majeure

1. Bien tester la/les feature(s) en local, en staging
2. Tester les features sur plusieurs environnements staging (à minima Deuxfleurs et Netlify)
  2.1 Tester desktop / mobile et navigateurs communs (FF, Edge, Safari, Chrome)
  2.2 Attention aux sites avec un darkmode
3. Demander (et attendre) une review sur la PR avant merge
4. Rechercher des effets de bord sur les sites avec override :
  4.1 Récupérer tous les sites en local (commande magique git clone tous les sites osuny)
  4.2 Ouvrir le dossier avec tous les sites dans VS Code
  4.3 Rechercher des références aux modifications (par exemple, changement d'un nom de classe dans le thème, ou d'un nom de partial)
  4.4 Noter les sites concernés pour les mettre à jour manuellement après le merge et avant la release
5. Une fois merge, déployer sur example.osuny.org
6. Re-tester
7. Si tout est ok et que les sites en étape 4 sont OK et en ligne, faire une release



