---
title: "Développer un site"
weight: 3
---

*Intégrer un site Web accessible et sobre avec Osuny, pour produire votre propre site Web*

![](/images/home/website.jpg)

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
yarn osuny dev
```

## Utiliser des données d'exemple

Vous pouvez utiliser des données d'exemple, présentant l'ensemble des cas possibles avec Osuny, ce qui vous permet de travailler sur l'apparence du site avant même d'avoir publié du contenu. Bien entendu, vous pouvez repasser très simplement sur vos données réelles dès qu'elles sont disponibles, et alterner en fonction de vos besoins.

Pour installer le contenu d'exemple, on utilise la commande :

```bash
yarn osuny setup-example
```

Pour travailler sur le site avec le contenu d'exemple, on utilise la commande :

```bash
yarn osuny server-example
```

WARNING : quelque chose ne fonctionne pas avec cette commande, il faut la réparer.

## Et maintenant ?

L'idée générale pour développer votre site sur la base d'Osuny est de procéder en suivant les étapes suivantes.


{{% steps %}}

### config.yaml

Configurer tout ce qui peut l'être dans le fichier `/config/_default/config.yaml`.
Cela permet par exemple de définir la position du fil d'ariane, du résumé, la longueur des troncatures ou le choix d'une mise en page en liste ou en grille des actualités.
Quand quelque chose n'est pas personnalisable dans le fichier config.yaml, on passe à l'étape 2.

### configuration.sass

Le fichier `/assets/sass/_configuration.sass` est destiné à recevoir des définitions de variables qui vont être utilisées par le thème.
Les variables disponibles sont disponibles ici :
https://github.com/noesya/osuny-hugo-theme-aaa/blob/main/config.yaml

### style.sass

Quand une modification n'est pas faisable avec les variables, il faut écrire du code Sass dans le fichier `/assets/sass/_style.sass`. 
Pour écrire les sélecteurs CSS, vous pouvez vous appuyer sur le DOM ou aller regarder dans les fichiers du thème.
Il faut, autant que possible, utiliser les helpers et les conventions du thème (`px2rem(20)`, `@include media-breakpoint-up(desktop)`, etc).
Cela permet de maintenir la cohérence et d'éviter les effets de bord, particulièrement liés au responsive.

### layouts

Enfin, quand le style ne suffit pas, tout le balisage HTML peut être modifié en dupliquant les fichiers du thème dans le dossier `/layouts`. 
Attention, cela doit être fait en dernier recours, parce qu'en faisant cela vous ne bénéficiez plus des mises à jour du thème.
Lorsque le thème évoluera, il faut mettre à jour vos propres fichiers HTML pour rester compatibles.
Évidemment, vos modifications HTML doivent prendre en compte les problématiques d'accessibilité et de sobriété de la même manière que le thème.

{{% /steps %}}