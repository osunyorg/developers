---
title: Publications
weight: 4
---

## Pages d'index

Hugo génère par défaut des pages d'index pour chaque section (type).


### Comment ne pas publier des index ?

Dans le cas où on ne souhaite pas afficher la page d'index, il faut modifier ou ajouter un static d'index (`_index.html`) de la section avec ces paramètres de `build` :

```yaml
---
build:
  list: always
  publishResources: true
  render: never
---
```

`list: always` : permet de lister la section (par exemple dans le plan du site)
`publishResources: true` : permet de publier les enfants
`render: never` : ne construit pas la page (rend une 404)

[Voir la documention hugo](https://gohugo.io/content-management/build-options/)


#### Permalink 

Il faut également ré-écrire les permalinks des enfants de la section concernée :

Exemple d'un projet : 

```yaml (content/fr/projects/2012-ecole-maternelle-structure-bois.html)
url: "/projets/2012-ecole-maternelle-structure-bois/"
slug: "ecole-maternelle-structure-bois"
meta:
  hugo:
    permalink: "/projets/2012-ecole-maternelle-structure-bois/"
    path: "/projects/2012-ecole-maternelle-structure-bois"
    file: "content/fr/projects/2012-ecole-maternelle-structure-bois.html"
    slug: "ecole-maternelle-structure-bois"
```

Doit être : 

```yaml
url: "/2012-ecole-maternelle-structure-bois/"
slug: "ecole-maternelle-structure-bois"
meta:
  hugo:
    permalink: "/2012-ecole-maternelle-structure-bois/"
    path: "/projects/2012-ecole-maternelle-structure-bois"
    file: "content/fr/projects/2012-ecole-maternelle-structure-bois.html"
    slug: "ecole-maternelle-structure-bois"
```

