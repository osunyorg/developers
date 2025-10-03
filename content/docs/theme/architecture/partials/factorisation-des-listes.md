---
title: Factorisation des listes
---

## Problématique

Il y a trop de fichiers html pour chaque collection.

## Cas concret

Il existe une quinzaine de collections (`type`), pour chacune de ces collections, on multiplie les fichiers html de liste et les items. À chaque ajout d'un nouveau `type` le thème grossit de plus de 15 partiels `html`. Si on fait des fichiers par `layout`, cela décuple encore le nombre de fichiers.

## Situation idéale

Les listes des objets sont toutes très similaires. Factoriser et ajouter des passerelles permet de solidifier et garantir la bonne sémantique `html`.

## Solution proposée

Un partiel de liste par défaut, capable de gérer le type d'objet et le layout désiré.

```html {filename="jobs/section.html"}
<div class="container">
  {{ partial "jobs/fragments/list.html" (dict
    "_context" .
    "type" "jobs"
    "items" .Paginator.Pages
    "layout" site.Params.jobs.index.layout
    "options" site.Params.jobs.index.options
    "heading_level" 2
  ) }}
</div>
```

```html {filename="commons/list/list.html"}
{{ $type := .type }}
{{ $layout := .layout }}
{{ $items := .items }}
{{ $heading_level := .heading_level }}
{{ $options := .options }}
{{ $item_partial := (printf "%s/fragments/list/item.html" $type )}}

<ul class="{{ $type }} {{ $type }}--{{- $layout }}">
  {{ range $items }}
    <li>
      {{ partial $item_partial (dict
        "item" .
        "heading_level" $heading_level
        "layout" $layout
        "options" $options
      )}}
    </li>
  {{ end }}
</ul>
```

Une passerelle en cas d'override

```html {filename="jobs/fragments/list.html"}
{{ partial "commons/list/list.html" . }}
```


Un fichier `item` pour chaque `type`

```html {filename="jobs/fragments/list/item.html"}
{{ $job := .item }}
{{ $layout := .layout | default "list" }}
{{ $heading_level := .heading_level }}
{{ $with_more := .with_more | default site.Params.jobs.index.options.with_more }}
{{ $options := .options }}
{{ with $job }}
  {{ $class := "job" }}
  {{ if and .Params.image $options.image }}
    {{ $class = printf "%s job--with-image" $class }}
    {{ $image_direction := partial "GetImageDirection" .Params.image }}
    {{ $class = printf "%s %s" $class $image_direction }}
  {{ end }}

  {{ $dates := .Params.dates }}

  <article class="{{ $class }}" 
      itemscope itemtype="https://schema.org/JobPosting">
    <div class="job-content">
      {{ partial "jobs/partials/job/heading.html" (dict
          "item" .
          "options" $options
          "level" $heading_level
          "itemprop" "title"
        )}}

      {{ if and $options.summary .Params.summary }}
        <div class="job-description" itemprop="abstract">
          {{ partial "jobs/fragments/list/item/summary.html" . }}
        </div>
      {{ end }}

      {{ if $options.dates }}
        {{ partial "jobs/fragments/list/item/dates.html" . }}
      {{ end }}

      {{ if $options.categories }}
        {{ partial "commons/categories.html" ( dict
          "context" .
          "kind" "job"
        )}}
      {{- end -}}

      {{ if $with_more }}
        {{ partial "commons/list/item/more.html" . }}
      {{- end -}}
    </div>

    {{ if $options.image }}
      {{ partial "jobs/fragments/list/item/media.html" (dict 
          "image" $job.Params.image
          "layout" $layout
        )}}
    {{ end }}
  </article>
{{ end }}
```

Testable sur cette [branche](https://github.com/osunyorg/theme/tree/v8-files-tree-exploration).
