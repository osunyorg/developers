---
title: Structure
weight: 1
---

Ce document fixe la structure des fichiers HTML du thème Osuny.

## Style

- Indentation = 2 espaces
- utilisation du tiret `{{- -}}` lorsque l'on veut supprimer des espaces entres des éléments d'inline qui causeraient des espaces visuels. Le HTML étant minifié il n'est pas nécessaire d'en mettre partout.

```
{{ if something }}
  something
{{ end }}

<p>
  {{- if something -}}
    <span>something</span>
  {{- end -}}
</p>
```


### Nommage et appels des partiels


#### Partiels normaux

Les partiels normaux sont ceux qui génèrent du html.

##### Nommage

Les noms des fichiers s'écrivent en lowercase et tiret (hyphen `-`) avec l'extension `.html`.

Par exemple : `image-figure.html`.

##### Appels

Appel d'un partiel avec un seul paramètre :

```
{{ partial "jobs/single/image.html" .Params.image }}
```

Appel d'un partiel avec plusieurs paramètre en utilisant `dict` :

```
{{ partial "contents/list.html" (dict
  "context" .
  "contents" .Params.contents
) }}
```

#### Partiels helpers

##### Nommage

Les helpers sont les partiels qui renvoient une valeur. Ils incluent systèmatiquement un `{{ return }}`.

Les noms des fichiers helpers s'écrivent en CamelCase avec l'extension `.html`. Les helpers sont des actions : `Get`, `Is`, `Has`... le nom du fichier doit toujours contenir un verbe, sinon c'est un partiel simple qui ne dit pas son nom.

##### Appels

{{< callout type="warning" >}}
L'appel d'un partiel helper s'écrie sans l'extension `.html` 
{{< /callout >}}

```
{{ $transcription := partial "PrepareHTML" .transcription }}
```

## Précision sémantique

{{< callout type="warning" >}}
  Afin d'utiliser des termes courants dans Hugo, on emploie layouts et partials.
  Cela a l'avantage d'être familier pour les devs Hugo, et l'inconvénient de désigner des dossiers différents avec le même nom.
{{< /callout >}}

| Terme | Hugo | Osuny |
| - | - | - |
| `layouts` | Fichiers HTML utilisés par le site | Fichiers HTML utilisés pour mettre en page possibles des objets (en cartes, en grille...) |
| `partials` | Composants utilisés par les fichiers natifs | Composants Osuny utilisés par les fichiers spécifiques Osuny |

## Un dossier pour chaque objet

{{< filetree/container >}}
  {{< filetree/folder name="layouts/_partials" >}}
    {{< filetree/folder name="jobs" state="open" >}}
      {{< filetree/folder name="partials" state="closed" >}}{{< /filetree/folder >}}
      {{< filetree/folder name="section" state="closed" >}}{{< /filetree/folder >}}
      {{< filetree/folder name="single" state="closed" >}}{{< /filetree/folder >}}
      {{< filetree/file name="section.html" >}}
      {{< filetree/file name="single.html" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

{{< callout type="info" >}}
  Tous les fichiers pour représenter les jobmes sont dans le dossier `layouts/_partials/jobs`
{{< /callout >}}

Les objets Osuny (`organizations`, `posts`, `pages`, `events`...) peuvent s'afficher de 3 façons : dans une section, dans un bloc ou dans une page. 
Dans la section et dans le bloc, il s'agit de lister des objets en utilisant une mise en page (`layout`) et des options.
Les fichiers à la racine du dossier layouts d'Hugo, dans `/layouts` (attention, pas les layouts Osuny !) suivent la logique de Hugo (par exemple `list.html` et `single.html`) et appellent les partials Osuny.

## Section

{{< filetree/container >}}
  {{< filetree/folder name="layouts/_partials" >}}
    {{< filetree/folder name="jobs" state="open" >}}
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
  {{< filetree/folder name="layouts/_partials" >}}
    {{< filetree/folder name="jobs" state="open" >}}
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
Le dossier `single` contient des objets nécessaires à l'affichage de la page.
Ce sont souvent de simple “passe-plats” vers des partiels plus communs.

## Partials

{{< filetree/container >}}
  {{< filetree/folder name="layouts/_partials" >}}
    {{< filetree/folder name="jobs" state="open" >}}
      {{< filetree/folder name="partials" state="open" >}}
        {{< filetree/folder name="job" state="open" >}}
          {{< filetree/file name="dates.html" >}}
          {{< filetree/file name="heading.html" >}}
          {{< filetree/file name="media.html" >}}
          {{< filetree/file name="summary.html" >}}
        {{< /filetree/folder >}}
        {{< filetree/file name="job.html" >}}
        {{< filetree/file name="jobs.html" >}}
      {{< /filetree/folder >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

{{< callout type="info" >}}
  Tous les fichiers utilisés à la fois par les blocs, par la section et par la single
{{< /callout >}}

Le partiel `jobs.html` est une liste de formations paginée avec options, utilisée dans la section et dans les catégories de formations.
