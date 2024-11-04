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

### Fichiers

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


## Comment connecter aux catégories ?

### Dans les objets



### Dans les blocs
