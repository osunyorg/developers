---
title: Blocs
weight: 6
aliases: 
- /docs/website/coder-le-side/blocs/
---

## Les blocs



## Manipuler les blocs dans le contenu de la page

Il est possible d'exclure des blocs du contenu d'une page, ce qui permet notamment de les replacer ailleurs, pour les besoins du design.

### Exclusion

Pour ne pas afficher un ou plusieurs blocs en particulier, il faut leur ajouter une class supplémentaire dans le back-office, par exemple in-hero (qui devient dans le fichier statique block-class-in-hero), puis ajouter à la configuration du site (ou du sous-thème) ceci :

```yml {filename="config.yml"}
  blocks:
    ignored:
      - block-class-in-hero
```

Tous les blocs avec cette classe ne s'afficheront plus dans le contenu des pages grâce à un filtre dans la liste des blocs de chaque page :

```html {filename="partials/content/list.html"}
{{ if and (eq .kind "block") (not (in site.Params.hero.blocks .html_class) )}}
```

### Déplacement

Cela permet d'afficher ces blocs ailleurs : organisations ou appel à action dans un hero, après la navigation dans une actualité, etc.

Dans le cas de la Gaîté Lyrique, nous avons besoin d'afficher certains types de blocs dans le hero de certaines pages.

Dans un premier temps, on vérifie la présence de blocs dans la page, dans un hook ou dans un override du hero :

```html
{{ with .Params.contents }}
  {{ partial "blocks/ignored" . }}
{{ end }}
```

**NB :** dans le cas d'un override, c'est `.context.Params.contents` qui contient les données recherchées.

Dans `partials/blocks/ignored.html`, un fichier créé spécifiquement sur un site et non sur le thème Osuny, on fait ensuite l'inverse de l'exclusion, en cherchant les blocs dont le paramètre html_class correspond à la liste indiquée en configuration. On peut cibler un certain type de blocs uniquement, afin d'éviter les mauvaises surprises, comme une galerie dans le hero :

```
<div class="blocks">
  {{ range $index, $content := . }}
    {{ if in site.Params.blocks.ignored .html_class }}
      {{ if or (eq .template "chapter") (eq .template "call_to_action") (eq .template "organizations") (eq .template "image") }}
        {{ $template := printf "blocks/templates/%s.html" .template }}
  
        {{ if templates.Exists ( printf "partials/%s" $template ) }}
          {{ partial $template (dict
            "block" $content
            "index" $index
          )}}
        {{ end }}
      {{ end }}
    {{ end }}
  {{ end }}
</div>
```
