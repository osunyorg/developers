---
title: Catégories
---

## Comment structurer les catégories ?

### Configuration Hugo

Définir les permaliens et les taxonomies.

```yaml {filename="config/_default/languages.yaml"}
fr:
  permalinks:
    events_categories: /agenda/:slug/
    posts_categories: /actualites/:slug/
  taxonomies:
    events_category: events_categories
    posts_category: posts_categories
```

### Organisation des fichiers

Stocker les fichiers dans le dossier de contenu.

{{< filetree/container >}}
  {{< filetree/folder name="content" >}}
    {{< filetree/folder name="fr" state="open" >}}
      {{< filetree/folder name="posts_categories" state="open" >}}
        {{< filetree/folder name="categorie-1" state="open" >}}
          {{< filetree/file name="_index.html" >}}
          {{< filetree/folder name="sous-categorie-1" state="open" >}}
            {{< filetree/file name="_index.html" >}}
          {{< /filetree/folder >}}
        {{< /filetree/folder >}}
        {{< filetree/folder name="categorie-2" state="closed" >}}{{< /filetree/folder >}}
        {{< filetree/file name="_index.html" >}}
      {{< /filetree/folder >}}
      {{< filetree/folder name="events_categories" state="open" >}}
        {{< filetree/folder name="categorie-1" state="closed" >}}{{< /filetree/folder >}}
        {{< filetree/folder name="categorie-2" state="closed" >}}{{< /filetree/folder >}}
        {{< filetree/file name="_index.html" >}}
      {{< /filetree/folder >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

### Structure d'un fichier de terme

```yaml{filename="content/fr/posts_categories/a-la-une/vie-etudiante/_index.html"}
title: >-
  Vie étudiante
url: "/fr/actualites/a-la-une/vie-etudiante/"
slug: "a-la-une/vie-etudiante"
meta:
  hugo:
    permalink: "/fr/actualites/a-la-une/vie-etudiante/"
    file: "content/fr/posts_categories/a-la-une/vie-etudiante/_index.html"
    path: "/a-la-une/vie-etudiante/"
    slug: "a-la-une/vie-etudiante"
  dates:
    created_at: 2024-05-03T11:11:20+02:00
    updated_at: 2024-09-29T14:40:43+02:00
parent: "/fr/actualites/a-la-une/"
```
https://developer.mozilla.org/fr/docs/Glossary/Slug

## Comment connecter aux catégories ?

Pour connecter un `post` à un `term` il faut lui donner la chaîne de `slug`.

```
posts_categories:
  - "offre-de-formation-animation-sociale-et-socioculturelle"
  - "a-la-une"
  - "a-la-une/vie-etudiante"
```

### Dans les blocs de liste

#### Bloc actualités en mode catégories

Example avec une sous-catégorie `Vie étudiante` enfant de `À la une`.

Dans le bloc on identifie la catégorie avec sa chaîne de `slug` dans la clé `category`.

```
  - kind: block
    template: posts
    title: >-
      Actualité - catégorie spécifique - Liste
    slug: >-
      actualite-categorie-specifique-liste
    ranks:
      self: 3
      children: 4
    data:
      mode: category
      category: "a-la-une/vie-etudiante"
```
