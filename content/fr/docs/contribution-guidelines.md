---
title: Règles de contribution
weight: 9
description: >
  Participer au commun
---

## Principes

Afin de permettre une bonne compréhension des développements et une maintenance facile, nous utilisons la [gestion sémantique de version](https://semver.org/lang/fr/), à la fois pour le thème et pour l'admin.

## Fonctionnements

### Contributions

Pour signaler un problème ou un souhait de fonctionnalité, créer une issue.

Les étapes pour contribuer sont :
1. Créer une branche nommée en fonction de ce qu'on fait, le plus simplement possible (en anglais, car les noms de branches n'ont ni espaces ni accents)
2. Commit en nommant les actions simplement
3. Faire une Pull Request (PR) avec un nom simple en français et référencer les issues en les mentionnant dans la description (avec close ou fix si c'est le cas)
4. Demander la review
5. Faire les modifs demandées par les reviewers
6. Quand la PR est approuvée, la fusionner (merge) pour qu'elle parte en production automatiquement

La contribution directement dans main est bloquée.

En revanche, les personnes de l'équipe admin peuvent auto-valider une PR, et doivent le faire uniquement pour les bugfix.

### Versions

L'équipe cœur crée des releases directement sur Github, avec le système de génération automatique de changelog.


## Analyse de pratiques

### Github

La [documentation](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository) explique comment générer des releases, et comment utiliser la génération automatique de descriptions. Cela donne ça pour la [v1.1.0](https://github.com/noesya/osuny/releases/tag/v1.1.0).

### Hugo 

[Hugo](https://github.com/gohugoio/hugo) est versionné en [v0.107.0](https://github.com/gohugoio/hugo/releases/tag/v0.107.0), en suivant ce [guide de contribution](https://github.com/gohugoio/hugo/blob/master/CONTRIBUTING.md).

Le système utilisé est développé maison, et s'appelle hugoreleaser.
- https://github.com/gohugoio/hugo/blob/master/hugoreleaser.env
- https://github.com/gohugoio/hugo/blob/master/hugoreleaser.toml


### Ruby on Rails

[Rails](https://github.com/rails/rails) est versionné en [v7.0.4](https://github.com/rails/rails/releases/tag/v7.0.4), en suivant ce [guide de contribution](https://edgeguides.rubyonrails.org/contributing_to_ruby_on_rails.html).

Rails est une gem, et utilise donc le [système de release intégré aux gems](https://guides.rubygems.org/releasing-rubygems/).

Les Pull Requests sont [très petites](https://github.com/rails/rails/pull/46517), avec souvent 1 seul commit.

### Devise

Idem Rails.

### Sympa

[Sympa](https://github.com/sympa-community/sympa) est versionné en [6.2.70](https://github.com/sympa-community/sympa/releases/tag/6.2.70)

