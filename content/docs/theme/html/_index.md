---
title: HTML
---

## Définitions

### Les Sections

Les `sections` sont les types de contenu, l'équivalent des `post-type` sur wordpress.

### Les Items

Un `item` représente une section présente dans une page, par exemple dans un bloc de liste de projets, chaque projet est un `item`.

Les `items` doivent comporter la classe `[section]-item`.

```html
<article class="post-item">
    [...]
</article>
```

### Les Pages

Une `page` représente la page d'un contenu section. L'équivalent d'un `single` dans wordpress.

Les `pages` doivent comporter la bodyclass `[section au pluriel]__page`.

```html
<body class="posts__page">
    [...]
</body>
```

### Les Index

Une `list-pages` représente la page qui groupe et pagine les différents contenu d'une section. C'est l'équivalent de l'index 

Les `list-pages` doivent comporter la bodyclass `[section au pluriel]__section`.

```html
<body class="posts__section">
    [...]
</body>
```