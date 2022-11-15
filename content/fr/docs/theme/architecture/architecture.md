---
title: "Architecture du thème"
weight: 1
description: >
  Comprendre l'organisation générale du thème
---

## HTML

Le thème suit l'organisation des thèmes Hugo, comme expliqué dans la documentation sur  [gohugo.io/documentation](https://gohugo.io/documentation/).

## CSS

Les styles s'empilent dans la logique suivante.
1. d'abord, le design système fixe les mécaniques générales : marges, grilles, typos...
2. ensuite, les blocs fixent les mécaniques spécifiques par type de contenu, quel que soit le contexte.
3. enfin, les sections définissent les mécaniques contextuelles.

Pour être faciles à maintenir, il faut définir les règles le plus haut possible dans cette pile : 
1. tout en haut, dans `design-system`, par défaut
2. si ce n'est pas lié au design système, dans les `blocks`
3. si ça ne concerne pas le design système ni un bloc partout, dans les `sections`

**Exemple** Si une taille de typo concerne :
1. tous les h2 -> `design-system/typography.sass`
2. tous les h2 des blocs timeline -> `blocks/timeline.sass`
3. uniquement les h2 de la page d'une personne -> `sections/persons.sass`

[SASS](https://sass-lang.com) est la syntaxe la plus légère pour le dev, donc nous l'adoptons.
Il n'y a pas d'impact de la syntaxe sur la rétrocompatibilité du fait de la compilation en CSS.

L'arborescence des fichiers .sass est la suivante :

```
assets
  _theme
    configuration.sass
    blocks
      chapter.sass
      gallery.sass
      timeline.sass
      …
    design-system
      a11y.sass
      breadcrumb.sass
      grid.sass
      header.sass
      typography.sass
      nav.sass
      footer.sass
    sections
      posts.sass
      programs.sass
      persons.sass
      …
```

## JS

L'objectif général est de miniser les dépendances JavaScript.
Cela permet de :
- faciliter la maintenance
- alléger le js
- diminuer les manipulations sur le DOM

A l'abandon de Bootstrap, nous avons recodé en JavaScript vanille :
- menu
- scrollspy 
- dropdown

Certains blocs nécessitent du JavaScript spécifique :
- chiffres clés
- timeline