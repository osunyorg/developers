---
title: Cloudflare
---

## Problème d'intégrité du JS

Cloudflare n'est pas un serveur, mais il cause le problème suivant, lié à l'hébergement.

Cloudflare minifie les fichiers, ce qui invalide le sha256, et bloque le chargement [Issue 36](https://github.com/osunyorg/theme/issues/36).

Si l'on ne peut pas couper ce fonctionnement, on peut ajouter .min.js à la fin du fichier.
Si quelqu'un arrive à faire faire cela à Hugo, super !

En attendant, il suffit de supprimer le sha comme cela `layouts/partials/footer/js.html`

```html
{{ $js := resources.Get "js/main.js" | js.Build (dict "minify" hugo.IsProduction) | fingerprint }}
<script src="{{ $js.RelPermalink }}" integrity=""></script>
```