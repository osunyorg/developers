---
title: Démarrage rapide
weight: 1
description: >
  Tout pour faire un site avec Osuny
---

Pour faire un site avec Osuny, la solution la plus simple est de partir du template Github [osuny-hugo-template-AAA](https://github.com/noesya/osuny-hugo-template-AAA).
Ce template utilise le [thème osuny-hugo-theme-AAA](https://github.com/noesya/osuny-hugo-theme-AAA).
Le template propose une configuration de site à jour, avec Hugo, le thème et les scripts facilitant les mises à jour.
Comme les sites sont développés avec Hugo, il faut l'installer pour coder en local.

> Avant le template AAA, il y a eu un autre template, nommé [osuny-hugo-template](https://github.com/noesya/osuny-hugo-template), qui utilisait le thème [osuny-hugo-theme](https://github.com/noesya/osuny-hugo-theme). Ce template et ce thème sont obsolètes. Il a fait l'objet de plusieurs refontes, et a lui-même succédé au thème Jekyll, au début d'Osuny. La mention AAA se réfère à l'article [Qualité frontend : à la recherche du AAA](https://lab.noesya.coop/2022/qualite-front), publié sur le [Lab noesya](https://lab.noesya.coop).

## Installer Hugo

### Mac

Sur Mac, avec [Homebrew](https://brew.sh), il faut utiliser la commande :

```bash
brew install hugo
```

C'est la méthode que nous utilisons dans l'équipe [noesya](https://www.noesya.coop).
Pour d'autres méthodes, la [documentation officielle d'installation](https://gohugo.io/getting-started/installing/) est disponible sur le site [gohugo.io](https://gohugo.io).

### Windows

La méthode la plus simple pour installer Hugo sur Windows est d'utiliser un package manager, comme [Scoop](https://scoop.sh) ou [Chocolatey](https://chocolatey.org).

```bash
choco install hugo-extended
```

Voir la [documentation officielle d'installation](https://gohugo.io/installation/windows/) pour plus d'informations sur l'installation avec Windows.

## Installer Yarn

### Mac

Sur Mac, avec [Homebrew](https://brew.sh), il faut utiliser la commande :

```bash
brew install yarn
```

[Documentation officielle d'installation](https://yarnpkg.com/getting-started/install).

### Windows

Pour installer Yarn sur Windows, la méthode recommandé est d'utiliser NPM inclu avec l'installation de Node.js.

```bash
npm install --global yarn
```

Pour plus d'informations sur l'installation de Yarn sur Windows, voir la [documentation officielle d'installation](https://classic.yarnpkg.com/en/docs/install).

## Créer le référentiel

Sur [la page du template](https://github.com/noesya/osuny-hugo-template-AAA), il faut cliquer sur le bouton "Use this template", puis donner un nom et valider.
Dans ce tutoriel, nous utiliserons le nom monreferentiel.

Une fois le référentiel créé, il faut le cloner en local.
Le thème est un sous-module git.
Pour cloner avec le thème, il faut utiliser la commande :

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

## Travailler avec des données d'exemple

Vous pouvez utiliser des données d'exemple, présentant l'ensemble des cas possibles avec Osuny, ce qui vous permet de travailler sur l'apparence du site avant même d'avoir publié du contenu.

Pour installer le contenu d'exemple, on utilise la commande :

```bash
yarn setup-example
```

Pour travailler sur le site avec le contenu d'exemple, on utilise la commande :

```bash
yarn server-example
```

## Autre option : ajouter le thème à un site Hugo existant

TODO traiter le passage en module Hugo

```bash
git submodule add https://github.com/noesya/osuny-hugo-theme-aaa.git themes/osuny-hugo-theme-aaa
yarn
```

Dans la configuration Hugo, définir le thème `osuny-hugo-theme-aaa`.

```bash
hugo server
```
