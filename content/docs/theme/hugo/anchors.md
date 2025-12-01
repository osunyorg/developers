---
title: Ancres
weight: 10
---

## Les liens d'ancre dans Hugo


On utilise la [méthode `anchorize`](https://gohugo.io/functions/urls/anchorize/) pour générer les ancres de la Table des matières. Mais en générant le html, l'ancre dans un attribute `href` est traîtée.

### Exemple du problème : 

```
{{ $anchorId := "Présentation" }}
{{ $anchorId = $anchorId | anchorize }} → "présentation"

<a href="#{{ $anchorId }}"> → <a href="#pr%c3%a9sentation>
<section id="{{ $anchorId }}"> → <section id="présentation>
```


### Solution :

On utilise le partiel `AnchorAttribute`

```{filename="partials/AnchorAttribute"}
{{ $anchor := anchorize . }}
{{ $attribute := printf "href=\"#%s\"" $anchor }}
{{ return (safeHTMLAttr $attribute) }}
```

Usage : 

```
<a {{ partial "AnchorAttribute" "Présentation" }}"> → <a href="#présentation>
```