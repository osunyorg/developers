---
title: Créer un thème "système de design"
description: >-
  Pour pouvoir utiliser le même système de design sur plusieurs sites
weight: 1
---

[Schéma](https://www.osuny.org/fonctionnalites/theme-osuny/schema-1/)

## Configurer son référentiel
### Créer un référentiel vide

Dans ce tutoriel, nous utiliserons le nom montheme. Il faut commencer par créer un référenciel vide pour le thème puis l'ajouter au référentiel Osuny déjà existant avec cette commande.

```bash
git submodule add git@github.com:monorganisation/montheme.git themes/montheme-hugo-theme
```

### Créer un fichier config

Dansl le thème, il faut créer un fichier `config.yaml`. Les paramètres principaux :

```bash
theme: osuny
params:
  logo:
    header: "/assets/images/votrelogo.svg"
    footer: "/assets/images/votrelogo.svg"
  seo:
    title:
      separator: « • »
```

Vous pouvez aussi y ajouter tout les paramètres dont vous avez besoin pour l'entiereté de vos sites.

## Créer vos dossiers

À présent il est possible de créer un dossier `assets`, ainsi qu'un dossier `montheme-hugo` qui doit directement être dans le dossier assets, et doit s'afficher ainsi à l'écran : `assets/montheme-hugo` . Il ensuite possible de créer un dossier `sass`, ainsi qu'un dossier `js` si nécessaire.

### Configurer les assets

Dans le dossier `sass`, il est à présent possible de créer les fichiers nécessaires, afin de définir les typographies, les styles et les configurations qui répondent aux besoins de l'entiereté des sites.

`_fonts.sass`
`_configuration.sass`
`_style.sass`
`theme.sass`

Le dernier fichier `_theme.sass` sera celui dans lequel les fichiers seront appelé. Il est nécessaire qu'il ait un autre nom que le fichier `main.sass` dans le referentiel Osuny.

> [!TIP] Astuce
> Dans la configuration, il faut garder les mentions !default afin de permettre les surcharges dans les sites. 

```sass
// Cette variable peut être modifiée dans le site
$header-dropdown-full: true !default

// Cette variable ne peut pas être modifiée
$header-dropdown-full: true
```

Dans le premier cas, la chaîne de configuration est 
```sass
// Dans le thème osuny
$header-dropdown-full: false !default 
// Dans le thème système de design 
$header-dropdown-full: true !default
// Dans le site
$header-dropdown-full: false
```

### Liaison des thèmes

Pour faire le lien entre le nouveau thème ainsi que le thème Osuny, dans le fichier `theme.sass`, il faudra que appeler le thème Osuny, ainsi que les utils, tel que :

```bash
@import "_theme/utils"
@import "_theme/hugo-osuny"
```
Puis appeler les fichiers nécessaires.

## Dans votre référentiel Osuny

Dans le dossier `config/_default` il faut aller dans le fichier `config.yaml` et remplacer le thème Osuny par le nouveau. La commande devrait se présenter ainsi :

```bash
theme: montheme-hugo-theme
```

Ensuite, dans le dossier `/assets/sass` il faut aller dans le fichier `main.sass` et changer le thème Osuny par le nouveau en indiquant le chemin de votre dossier, d'où l'importance d'associer le dossier assets de votre theme avec un nom tel que `montheme-hugo`, ainsi que de différencier le nom de votre fichier de celui du main. La commande devrait se présenter ainsi :

```bash
@import "montheme-hugo/theme"
```

## Vos logos et typos

Pour stocker vos fichiers typographiques, vos logos ainsi que vos images, il faut créer un dossier `static/assets` dans lequel il y aura les dossiers `fonts` et `images`.
