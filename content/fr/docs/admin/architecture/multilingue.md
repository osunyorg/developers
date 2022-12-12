---
title: Multilingue
weight: 3
description: >
  Parti-pris sur la gestion des langues
---

D'une façon générale, les langues se gèrent en 2 phases :
- l'internationalisation (i18n) qui vise à rendre possible les langues multiples
- la localisation (l10n) qui vise à intégrer une langue spécifique

Dans Osuny, le système a 2 facettes :
- l'admin en Ruby on Rails, qui permet d'écrire les contenus
- le site en Hugo, qui permet de lire les contenus

Il faut donc partir du multilingue [tel qu'il fonctionne dans les sites Hugo](https://gohugo.io/content-management/multilingual/) pour générer des fichiers corrects depuis Osuny.

## Hugo

### La configuration

Nous utilisons un fichier languages.yml
```
ar:
  languagedirection: rtl
  title: مدونتي
en:
  title: My blog
fr:
  title: Mon blogue
pt-pt:
  title: O meu blog
```

### Les contenus

Il y a 2 façons de gérer les contenus multilingues dans Hugo : 
- toutes les langues d'un contenu dans le même répertoire
- un répertoire par langue contenant tous les contenus

Nous utilisons le second système, avec une config de type :
```
fr:
  title: Site d'exemple Osuny
  contentDir: content/fr
en:
  title: Osuny example website
  contentDir: content/en
```

Chaque contenu doit être 

### Les clés de traduction

## Ruby on Rails

### Le paramétrage du site

### La traduction

### L'export statique