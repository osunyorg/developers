---
title: En cas de problème
weight: 10
description: >
  Répondre aux questions fréquentes
---

Si vous êtes perdus, venez sur le [Slack de Support Osuny](https://osunysupport.slack.com), on vous aidera avec plaisir !

## Problème d'intégrité du JS avec Cloudflare

Cloudflare minifie les fichiers, ce qui invalide le sha256, et bloque le chargement [Issue 36](https://github.com/noesya/osuny-hugo-theme-aaa/issues/36).

Si l'on ne peut pas couper ce fonctionnement, on peut ajouter .min.js à la fin du fichier.
Si quelqu'un arrive à faire faire cela à Hugo, super !

En attendant, il suffit de supprimer le sha comme cela `layouts/partials/footer/js.html`

```html
{{ $js := resources.Get "js/main.js" | js.Build (dict "minify" hugo.IsProduction) | fingerprint }}
<script src="{{ $js.RelPermalink }}" integrity=""></script>
```