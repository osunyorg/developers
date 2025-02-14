---
title: Options
---

## Le nœud `options`

Il faut généraliser l'emploi d'un nœud `options` qui contient toutes les options d'affichage.
Il ne faut ni show ni hide, mais le critère brut.
La valeur `true` indique qu'il faut l'afficher, la valeur `false` qu'il faut le masquer.

```YAML{filename="post"}
options:
  image: true
  summary: true
  category: true
  author: true
  date: true
```

Dans cet exemple, il faut tout afficher.

## La configuration du site

Le fichier de configuration doit utiliser les options par contexte, c'est à dire au moins une fois dans `index` et une fois dans `single`.

```YAML {filename="themes/osuny/config.yaml"}
  events:
    default_image: false
    date_format: ":date_long"
    truncate_description: 200 # Set to 0 to disable truncate
    index:
      options:
        categories: false
        author: false
        description: true
      layout: list # grid | list
    single:
      options:
        categories: true
        author: true
        description: true
```

Il y a des incohérences à résoudre :
- `category` ou `categories` ? `categories` est plus juste
- `description` ou `summary` ? `summary` est plus cohérent

Le `layout` n'est pas une option d'affichage, il va déterminer à quel partial on passe les options.
L'image par défaut `default_image` et le format de date `date_format` n'ont pas de raison de changer en fonction des contextes, c'est pour tout le site.
La troncature de description `truncate_description` devrait aussi passer pour tout le site, c'est une logique d'ensemble.

## Les blocs

Il faut utiliser aussi des nœuds options, que l'on peut passer tels quels aux partials, d'où l'importance de les unifier.

```YAML {filename="Bloc pages"}
  - kind: block
    template: pages
    data:
      main_description: false
      options:
        description: true
        image: false
      layout: grid
```

La variable `main_description` est liée au cas particulier de la page principale avec ses enfants, ce n'est pas une option d'affichage normale des pages.
Il faut remettre les valeurs au singulier, parce qu'on va la passer au partial.

```YAML {filename="Bloc posts"}
  - kind: block
    template: posts
    data:
      mode: selection
      options:
        image: false
        summary: false
        category: false
        author: false
        date: false
      layout: large
```

Pour `category` et `author`, singulier ou pluriel ? Pour l'instant on utilise le singulier.

## L'admin

Il faut ajouter la possibilité d'établir des valeurs par défaut, sous la forme de cases à cocher précochées à la création d'un bloc.

On passerait ainsi de
```ruby{filename="app/models/communication/block/template/post.rb"}
  has_component :hide_image, :boolean
```
à
```ruby{filename="app/models/communication/block/template/post.rb"}
  has_component :option_image, :boolean, default: true
```
Le préfixe option paraît une solution simple, mais peut-être faut-il structurer davantage en créant un objet `options`.

Cela pose un problème de migration des blocs existants, qu'il faut réaliser en conservant les choix des personnes.

## Le thème

```hugo{filename="themes/osuny/layouts/partials/posts/posts.html"}
  {{ range .Paginator.Pages }}
    {{  partial "posts/post.html"
        (dict
          "post" .
          "options" site.Params.posts.index.options
        )}}
  {{ end }}
```

