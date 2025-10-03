---
title: Transmission de variable au partiel
---

## Problématique

Quelles variables ou quel niveau de variable passer aux partiels ?

## Cas concret

Dans l'état de l'existant (v7.11.0) il y a plusieurs cas : 

- On passe uniquement le contexte à un partiel. À l'intérieur du partiel, il faut retrouver les données à afficher :
```html{filename="/layouts/partials/jobs/section.html"}
<div class="container">
  {{ partial "jobs/partials/jobs.html" . }}
  {{ partial "commons/pagination.html" . }}
</div>
```

- On passe les données à afficher et également le contexte :

```html{filename="/layouts/partials/jobs/section.html"}
{{ partial "contents/list.html" (dict
  "contents" .Params.contents
  "context" .
) }}
```

- On passe uniquement les données à afficher sans le contexte :

```html{filename="/layouts/partials/jobs/partials/jobs.html"}
{{ range $jobs }}
  <li>
    {{ partial "jobs/partials/job.html" (dict
        "job" .
        "heading_level" $heading_level
        "layout" $layout
        "options" $options
      )}}
  </li>
{{ end }}
```


## Situation idéale

Une seule façon de faire pour tous les partiels.


## Solution proposée

1. Isoler le context avec un `_` : `_context`
2. Passer explicitement les variables concernant le partiel en question

```html{filename="/layouts/partials/jobs/section.html"}
{{ partial "jobs/fragments/list.html" (dict
  "_context" .
  "type" "jobs"
  "items" .Paginator.Pages
  "layout" site.Params.jobs.index.layout
  "options" site.Params.jobs.index.options
  "heading_level" 2
) }}
```
