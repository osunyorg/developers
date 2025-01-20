---
title: HTML
---

## Les représentations des contenus

Les contenus peuvent être représentés sous différentes formes : dans une liste, dans une page, ou dans leur propre page.

### Les sections

Les `sections` sont les types de contenu, l'équivalent des `post-type` sur wordpress.

### Les items

Un `item` représente une section présente dans une page, par exemple dans un bloc de liste de projets, chaque projet est un `item`.

Les `items` doivent comporter la classe `[section]`.

```html
<article class="post">
    [...]
</article>
```

> Note : On peut envisager une classe `[section]-item` pour mieux distinguer les blocs `html`.

### Les pages

Une `page` représente la page d'un contenu section. L'équivalent d'un `single` dans wordpress.

Les `pages` doivent comporter la bodyclass `[section au pluriel]__page`.

```html
<body class="posts__page">
    [...]
</body>
```

### Les index

Une `list-pages` représente la page qui groupe et pagine les différents contenu d'une section. C'est l'équivalent de l'index 

Les `list-pages` doivent comporter la bodyclass `[section au pluriel]__section`.

```html
<body class="posts__section">
    [...]
</body>
```