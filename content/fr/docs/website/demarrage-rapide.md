---
title: Démarrage rapide
weight: 1
---

Pour faire un site avec Osuny, la solution la plus simple est de partir du template Github [osuny-hugo-template-AAA](https://github.com/noesya/osuny-hugo-template-AAA). 
Ce template utilise le thème osuny-hugo-theme-AAA(https://github.com/noesya/osuny-hugo-theme-AAA). 
Le template propose une configuration de site à jour, avec Hugo, le thème et les scripts facilitant les mises à jour. 
Comme les sites sont développés avec Hugo, il faut l'installer pour coder en local.

> Avant le template AAA, il y a eu un autre template, nommé [osuny-hugo-template](https://github.com/noesya/osuny-hugo-template), qui utilisait le thème [osuny-hugo-theme](https://github.com/noesya/osuny-hugo-theme). Ce template et ce thème sont obsolètes. Il a fait l'objet de plusieurs refontes, et a lui-même succédé au thème Jekyll, au début d'Osuny.

## Installer Hugo 

Sur Mac, avec [Homebrew](https://brew.sh), il faut utiliser la commande :
```bash
brew install hugo
```
C'est la méthode que nous utilisons dans l'équipe [noesya](https://www.noesya.coop). 
Pour d'autres méthodes, la [documentation officielle d'installation](https://gohugo.io/getting-started/installing/) est disponible sur le site [gohugo.io](https://gohugo.io).

## Installer Yarn

Sur Mac, avec [Homebrew](https://brew.sh), il faut utiliser la commande :
```bash
brew install yarn
```

[Documentation officielle d'installation](https://yarnpkg.com/getting-started/install).

## Cloner le référentiel

Le thème est un sous-module git. Pour cloner avec le thème, il faut utiliser la commande :
```bash
git clone git@github.com:monorganisation/monreferentiel.git --recurse-submodules
```

## Lancer le serveur

La première fois, on installe les dépendances :
```bash
cd monreferentiel
yarn install
```

Pour lancer le serveur, on utilise la commande :
```bash
hugo server
```

On peut aussi utiliser la commande :
```bash
yarn dev
```
