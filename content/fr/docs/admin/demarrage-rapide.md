---
title: Démarrage rapide
weight: 1
description: >
  Travailler en local sur le projet Ruby on Rails
---

## Prérequis

Pour travailler sur le projet, il faut :
1. un environnement multiple de Ruby (nous utilisons rbenv) pour utiliser le changement de version de Ruby indiqué dans le code (.ruby-version et Gemfile)
1. un Ruby correspondant à la version en cours
1. Postgresql à jour
1. Imagemagick
1. Node (nous utilisons NVM pour les versions avec .nvmrc)
1. Yarn

## Installation

1. Lancer l'installation de l'application Ruby on Rails avec
```bash
bin/setup
```

2. Paramétrer les variables d'environnement dans `config/application.yml`, sur la base de l'exemple `config/application.sample.yml`

3. Démarrer le serveur 
```bash
rails app:start
```

4. Pour utiliser des urls locales de type `http://demo.osuny:3000`, il faut paramétrer le fichiers hosts (sur Mac et Linux, `/etc/hosts`)
```text
127.0.0.1   demo.osuny 
```

## Mise en staging

Pour paramétrer le remote correspondant à Scalingo
```bash
git remote add staging git@ssh.osc-fr1.scalingo.com:osuny-staging.git
```

Pour envoyer en staging (sous réserve d'avoir les droits)
```bash
git push staging main
```

## Mise en production

Automatique avec Scalingo, quand les Pull Requests sont acceptées.
