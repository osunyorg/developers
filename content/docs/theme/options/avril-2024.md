---
title: Situation en avril 2024
weight: 10
---

Les paramètres d'affichage des différents objets (pages, actualités, événements, personnes...) manquent d'unification.
Certains paramètres sont globaux, pour tout le site, et d'autres sont locaux, au niveau d'un bloc.
Par ailleurs, les paramètres globaux dépendent des contextes : on peut vouloir afficher l'auteur d'un article sur la page de l'article (`single`), mais pas sur la page de liste des articles (`index`).

Une partie se situe dans le fichier de configuration :

```YAML {filename="themes/osuny/config.yaml"}
  events:
    default_image: false
    date_format: ":date_long"
    index:
      show_categories: false
      show_author: false
      show_description: true
      truncate_description: 200 # Set to 0 to disable truncate
      layout: list # grid | list
```

Un ajout récent (post) est groupé dans un nœud `options`, avec un doublon (`show`/`hide`).

```YAML {filename="themes/osuny/config.yaml"}
  posts:
    default_image: false
    date_format: ":date_long"
    index:
      show_categories: true
      show_author: false
      show_description: true
      truncate_description: 200 # Set to 0 to disable truncate
      layout: list # grid | list
      options:
        hide_image: false
        hide_summary: false
        hide_category: false
        hide_author: false
        hide_date: false
```

Une partie se situe dans les blocs eux-mêmes :

```YAML {filename="content/en/_index.html"}
  - kind: block
    template: pages
    data:
      show_main_description: false
      show_descriptions: true
      show_images: false
      layout: grid
```

Avec [l'ajout récent au bloc actualités](https://github.com/osunyorg/admin/pull/1726), on passe d'une syntaxe `show` à une syntaxe `hide` :

```YAML {filename="content/fr/_index.html"}
  - kind: block
    template: posts
    data:
      mode: selection
      hide_image: false
      hide_summary: false
      hide_category: false
      hide_author: false
      hide_date: false
      layout: large
```

Au niveau de l'admin, la question des `show` vs `hide` est mal traitée, il s'agit en fait d'une question de configuration par défaut.
Au début du développement d'Osuny, l'idée était de ne rien afficher, et de laisser les personnes ajouter des informations, d'où l'idée du `show`.
Ensuite, pour les options d'affichage des actualités, nous avons travaillé à l'envers : on affiche toutes les informations, et on choisit quoi masquer.
La réalité est que ça dépend des critères, et qu'il faut traiter ça avec des valeurs par défaut pertinentes.