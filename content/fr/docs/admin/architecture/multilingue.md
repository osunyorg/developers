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

L'aspect front du multilingue [est documenté dans la partie thème](/docs/theme/architecture/multilingue/).

## Le paramétrage du site

On force le choix d'au moins une langue.

## La traduction

## L'export statique

En cas de monolingue les `url` des fichiers statiques ne doivent PAS être préfixées de la langue.  
En cas de multilingue toutes les urls sont préfixées.  
On créé les contenus dans `content/:lang/`.
On crée les menus dans `data/menus/:lang/`.  
