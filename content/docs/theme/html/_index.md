---
title: HTML
---

## Les représentations des contenus

Les contenus peuvent être représentés sous différentes formes : dans une liste, dans une page, ou dans leur propre page.

### Les sections

Les `sections` sont les types de contenu, l'équivalent des `post-type` sur wordpress.

### Un `item`

Un `item` représente une section présente dans une page, par exemple dans un bloc de liste de projets, chaque projet est un `item`.

Les `items` doivent comporter la classe `[section]`.

```html
<article class="post">
    [...]
</article>
```

> Note : On peut envisager une classe `[section]-item` pour mieux distinguer les blocs `html`.

### Une `single`

Une `single` représente la page d'un contenu section.

Les `singles` doivent comporter la bodyclass `[section au pluriel]__page`.

```html
<body class="posts__page">
    [...]
</body>
```

### Une `list`

Une `list-pages` représente la page qui groupe et pagine les différents contenu d'une section. C'est l'équivalent de l'index 

Les `list-pages` doivent comporter la bodyclass `[section au pluriel]__section`.

```html
<body class="posts__section">
    [...]
</body>
```

### Arborescence des fichiers

Avec l'augmentation du nombre de partiel, pour éviter le monolithe (de grandes listes verticales), il faut étendre horizontalement. Pour être plus prévisible il faut facilement pouvoir trouver les partiels :

Par exemple pour les formations :
- dans le dossier `partials`
- dans le dossier `partials/programs` : tous les fichiers pour représenter les programmes 
- dans le dossier `partials/programs/single` : tous les fichiers liés à l'affiche d'une formation (par exemple : [Bibliothécaire](https://www.iut.u-bordeaux-montaigne.fr/formation/offre-de-formation/bibliothecaire/))
- dans le dossier `partials/programs/list` : tous les fichiers pour afficher des listes de formations, qu'il s'agisse de la page de l'[offre de formation](https://www.iut.u-bordeaux-montaigne.fr/formation/offre-de-formation/) ou d'un bloc formation
- dans le dossier `partials/programs/list/item` : tous les fichiers pour afficher une formation au sein d'une liste. Ce dossier peut contenir un format par layout (par exemple : `default.html`, `large.html`, `list.html`, `grid.html`...)

<img width="201" alt="image" src="https://github.com/user-attachments/assets/b441147a-078b-4def-996a-8674e5b9724c" />

