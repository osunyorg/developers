---
title: Règles de contribution
weight: 9
description: >
  Participer au commun
---

## Principes

Afin de permettre une bonne compréhension des développements et une maintenance facile, nous utilisons la [gestion sémantique de version](https://semver.org/lang/fr/), à la fois pour le thème et pour l'admin.

**Disclaimer : Ceci est un principe ! En pratique, on a du mal à le mettre en œuvre, mais ça va s'améliorer :)**


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

### Devise

Idem Rails.

### Sympa

[Sympa](https://github.com/sympa-community/sympa) est versionné en [6.2.70](https://github.com/sympa-community/sympa/releases/tag/6.2.70)