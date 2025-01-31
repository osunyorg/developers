---
title: HTML
---

## Les objets Osuny

Les objets Osuny (`organizations`, `posts`, `pages`, `events`...) peuvent s'afficher de 2 façons :
- dans une liste
- dans leur propre page
Ils sont traduits par des sections Hugo.

La liste existe dans 2 contextes différents : 
- la page native (la liste des actualités, l'agenda...)
- le bloc de l'objet

### Liste

#### Dans la page native

C'est la page native de Hugo pour lister les objets.
Elle est paginée.
Les objets `Page` sont un peu spéciales : elles sont à la fois une page et une liste (de ses enfants).

```html
<body class="posts__section">
    [...]
</body>
```

#### Dans un bloc

Les listes affichent des items.

```html
<article class="post">
  [...]
</article>
```

### Page de l'objet

Une `single` représente la page d'un contenu section.
Les `singles` doivent comporter la bodyclass `[section au pluriel]__page`.

```html
<body class="posts__page">
    [...]
</body>
```

## Arborescence des fichiers

Avec l'augmentation du nombre de partiels, pour éviter le monolithe (de grandes listes verticales), il faut étendre horizontalement. 
Pour être plus prévisible il faut facilement pouvoir trouver les partiels.

Les fichiers à la racine du dossier layouts suivent la logique de Hugo (`list.html` et `single.html`).


Le dossier des partials suit une logique propre à Osuny. 
D'abord, il y a un dossier pour chaque objets, au pluriel.

{{< filetree/container >}}
  {{< filetree/folder name="layouts/partials" >}}
    {{< filetree/folder name="programs" state="open" >}}
      {{< filetree/folder name="list" state="open" >}}
        {{< filetree/folder name="components" state="closed" >}}{{< /filetree/folder >}}
        {{< filetree/folder name="layouts" state="open" >}}
          {{< filetree/folder name="cards" state="open" >}}
            {{< filetree/file name="cards-item.html" >}}
            {{< filetree/file name="cards.html" >}}
          {{< /filetree/folder >}}
        {{< /filetree/folder >}}
      {{< /filetree/folder >}}
      {{< filetree/folder name="single" state="open" >}}
        {{< filetree/folder name="hero" state="closed" >}}{{< /filetree/folder >}}
      {{< /filetree/folder >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

| Dossier | Usage |
| - | - |
| `partials` | Tous les fichiers Osuny |
| `partials/programs` | Tous les fichiers pour représenter les programmes |
| `partials/programs/list` | Toutes les listes de formations, qu'il s'agisse de la page de l'[offre de formation](https://www.iut.u-bordeaux-montaigne.fr/formation/offre-de-formation/) ou d'un bloc formations |
| `partials/programs/list/components` | Les composants utilisés dans les items |
| `partials/programs/list/layouts` | Toutes les mises en page de listes de programmes |
| `partials/programs/list/layouts/cards` | La liste et l'item de la mise en page en cartes  |
| `partials/programs/list/hero` | L'entête de la page native  |
| `partials/programs/single` | Tous les fichiers liés à l'affichage d'une formation (par exemple : [Bibliothécaire](https://www.iut.u-bordeaux-montaigne.fr/formation/offre-de-formation/bibliothecaire/)) |
| `partials/programs/single/hero` | Tous les fichiers liés à l'affichage d'une formation (par exemple : [Bibliothécaire](https://www.iut.u-bordeaux-montaigne.fr/formation/offre-de-formation/bibliothecaire/)) |
