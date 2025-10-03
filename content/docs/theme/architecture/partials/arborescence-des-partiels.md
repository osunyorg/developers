---
title: Arborescence des partiels
---

## Problématique

Les path des fichiers sont confus.

## Cas concret

La liste des jobs est `partials/jobs/partials/jobs.html`. Deux fois `partials` et deux fois `jobs`...

## Situation idéale

Un chemin identifiable facilement et une pattern dans le nom des fichiers.

## Solution proposée

1. Remplacer `partials/jobs/partials` par `partials/jobs/fragments`.
2. Remplacer le fichier de liste `jobs.html` par `list.html`
2. Remplacer le fichier de l'item `job.html` par `item.html`

`partials/jobs/partials/jobs.html` devient `partials/jobs/fragments/list.html`
`partials/jobs/fragments/list/item.html`
`partials/jobs/fragments/list/item/heading.html`

{{< filetree/container >}}
  {{< filetree/folder name="layouts/partials" >}}
    {{< filetree/folder name="jobs" state="open" >}}
      {{< filetree/folder name="fragments" state="open" >}}
        {{< filetree/folder name="commons" state="open" >}}
          {{< filetree/file name="logo.html" >}}
        {{< /filetree/folder >}}
        {{< filetree/folder name="list" state="open" >}}
          {{< filetree/file name="item.html" >}}
          {{< filetree/folder name="item" state="open" >}}
            {{< filetree/file name="heading.html" >}}
            {{< filetree/file name="summary.html" >}}
          {{< /filetree/folder >}}
        {{< /filetree/folder >}}
        {{< filetree/file name="list.html" >}}

      {{< /filetree/folder >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}