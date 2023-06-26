---
title: Créer un thème "système de design"
description: >-
  Pour pouvoir utiliser le même système de design sur plusieurs sites
---

[Schéma](https://www.osuny.org/fonctionnalites/theme-osuny/schema-1/)

## Configurer son référenciel
### Créer un référentiel vide

Dans ce tutoriel, nous utiliserons le nom montheme. Commencez par créer un référenciel vide pour votre thème puis ajoutez le à votre référentiel Osuny déjà existant avec cette commande. 

```bash
git submodule add git@github.com:monorganisation/montheme.git themes/montheme-hugo-theme
```

### Créer un fichier config

Dans votre thème créez un fichier `config.yaml`. Vous pouvez déjà y mettre ces informations :

`theme: osuny-hugo-theme-aaa
params:
  logo:
    header: "/assets/images/votrelogo.svg"
    footer: "/assets/images/votrelogo.svg"
  seo:
    title:
      separator: « • »`

Vous pouvez aussi y ajouter tout les paramètres dont vous avez besoin pour l'entiereté de vos sites.

## Créer vos dossiers

Vous pouvez à présent créer un dossier `assets`, ainsi qu'un dossier `montheme-hugo` qui se trouve directement dans votre dossier assets, et doit s'afficher ainsi à l'écran : `assets/montheme-hugo` . Vous pouvez ensuite créer un dossier `sass`, ainsi qu'un dossier `js` si nécessaire. 

### Configurer les assets

Dans votre dossier `sass`, vous pouvez à présent créer les fichiers dont vous avez besoin, afin de définir les typographies, les styles et les configurations dont vous avez besoin pour tout vos sites.

`_fonts.sass`
`_configuration.sass`
`_style.sass`
`theme.sass`

Le dernier fichier `_theme.sass` sera celui dans lequel vous importerez vos autres fichiers, et il est nécessaire qu'il ait un autre nom que le fichier `main.sass` dans votre referentiel Osuny. 

### Liaison des thèmes

Pour faire le lien entre votre theme ainsi que le theme Osuny, dans votre fichier `theme.sass`, il faudra que vous importiez le theme Osuny, ainsi que les utils, tel que :

`@import "_theme/utils"
@import "_fonts"
@import "_configuration"
@import "_theme/hugo-osuny"
@import "_style"`

Ensuite, dans votre referentiel, vous allez devoir indiquer que vous voulez utiliser votre thème.

## Dans votre referentiel Osuny

Dans le dossier `config/_default` allez dans votre fichier `config.yaml` et remplacez le theme par le votre. La commande devrait se présenter ainsi :

```theme: montheme-hugo-theme```

Ensuite, dans le dossier `/assets/sass` allez dans votre fichier `main.sass`et changez le thème par le votre en indiquant le chemin de votre dossier, d'où l'importance d'associer le dossier assets de votre theme avec un nom tel que `montheme-hugo`, ainsi que de différencier le nom de votre fichier de celui du main. La commande devrait se présenter ainsi :

```@import "montheme-hugo/theme"```

## Vos logos et typos

Vous pouvez maintenant créer un dossier `static/assets` dans lequel vous aurez les dossiers `fonts` et `images`. Vous pourrez y mettre vos fichiers fonts ainsi que vos images et logos dont vous aurez besoin pour tout vos sites.
