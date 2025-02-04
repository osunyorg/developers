---
title: Structure des fichiers HTML
---

Ce document fixe la structure des fichiers HTML du thème Osuny.

## Un dossier pour chaque objet

{{< filetree/container >}}
  {{< filetree/folder name="layouts/partials" >}}
    {{< filetree/folder name="programs" state="open" >}}
      {{< filetree/folder name="partials" state="closed" >}}{{< /filetree/folder >}}
      {{< filetree/folder name="section" state="closed" >}}{{< /filetree/folder >}}
      {{< filetree/file name="section.html" >}}
      {{< filetree/folder name="single" state="closed" >}}{{< /filetree/folder >}}
      {{< filetree/file name="single.html" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

{{< callout type="info" >}}
  Tous les fichiers pour représenter les programmes sont dans le dossier `layouts/partials/programs`
{{< /callout >}}

Les objets Osuny (`organizations`, `posts`, `pages`, `events`...) peuvent s'afficher de 3 façons : dans une section, dans un bloc ou dans une page. 
Dans la section et dans le bloc, il s'agit de lister des objets en utilisant une mise en page (`layout`) et des options.
Les fichiers à la racine du dossier layouts suivent la logique de Hugo (`list.html` et `single.html`), et appellent les partials Osuny.

## Section

{{< filetree/container >}}
  {{< filetree/folder name="layouts/partials" >}}
    {{< filetree/folder name="programs" state="open" >}}
      {{< filetree/folder name="section" state="open" >}}
        {{< filetree/file name="hero.html" >}}
      {{< /filetree/folder >}}
      {{< filetree/file name="section.html" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

{{< callout type="info" >}}
  Tous les fichiers liés à l'affichage de la page de l'[offre de formation](https://www.iut.u-bordeaux-montaigne.fr/formation/offre-de-formation/)
{{< /callout >}}

C'est la page native de Hugo pour lister les objets d'un type.
Elle est paginée.
Certains objets, comme `Page`, sont un peu spéciaux : ils sont à la fois une page et une liste (de ses enfants).
Ce distingo est fait dans les fichiers natifs Hugo, à la racine de `layouts`.
La section est aussi utilisée par les taxonomies, pour afficher une catégorie de formations par exemple.

## Single

{{< filetree/container >}}
  {{< filetree/folder name="layouts/partials" >}}
    {{< filetree/folder name="programs" state="open" >}}
      {{< filetree/folder name="single" state="open" >}}
        {{< filetree/file name="hero.html" >}}
      {{< /filetree/folder >}}
      {{< filetree/file name="single.html" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

{{< callout type="info" >}}
  Tous les fichiers liés à l'affichage d'une formation (par exemple : [Bibliothécaire](https://www.iut.u-bordeaux-montaigne.fr/formation/offre-de-formation/bibliothecaire/))
{{< /callout >}}

Une `single` représente la page d'un objet.
Le dossier `partials` est optionnel, et doit contenir des fichiers permettant l'affichage de la single.

## Partials

{{< filetree/container >}}
  {{< filetree/folder name="layouts/partials" >}}
    {{< filetree/folder name="programs" state="open" >}}
      {{< filetree/folder name="partials" state="open" >}}
        {{< filetree/folder name="layouts" state="open" >}}
          {{< filetree/folder name="cards" state="open" >}}
            {{< filetree/file name="cards-item.html" >}}
            {{< filetree/file name="cards.html" >}}
          {{< /filetree/folder >}}
        {{< /filetree/folder >}}
      {{< /filetree/folder >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

{{< callout type="info" >}}
  Tous les fichiers utilisés à la fois par les blocs, par la section et par la single
{{< /callout >}}

Le dossier partials contient toujours un dossier layouts, avec un dossier par layout.
Les fichiers `cards.html` et `cards-item.html` pourraient s'appeler `list.html` et `item.html`, mais cela causerait des onglets qui ont tous le même nom dans l'éditeur de code. 