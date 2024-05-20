---
title: "Icônes"
weight: 10
description: >
  Comment utiliser les icônes dans son site.
---

## Police d'icônes

Le thème s'appuie sur une police d'icônes (font icons). Vous trouverez les icônes disponibles dans le [figma du thème](https://www.figma.com/design/tQh3oLfNwSt8aoVDuUjQt0/Th%C3%A8me-Osuny?node-id=513-1292).

## Utilitaire d'icônes

Vous trouverez les codes des icônes du thème dans `themes/osuny-hugo-theme-aaa/assets/sass/_theme/configuration/icons.sass`

Pour insérer une icône en `sass`, utilisez le mixin `icon`.

### Mixin `icon`

| Paramètres | Détail | Exemple | Valeur par défaut |
| ---------- | -------------- | ------ | ------- |
| $icon-name | nom de l'icône | `arrow-right` |
| $pseudo-element | Ajoute l'icône en `::before` ou `::after` cela permet de choisir si l'icône vient avant ou après l'élément visé | `before` ou `after` | `before` |
| $non-breaking  | Ajoute un espace insécable avant le caractère d'icône | `true` ou `false` | `false` |

```{filename="themes/osuny-hugo-theme-aaa/assets/sass/_theme/utils/icons.sass"}
@mixin icon($icon-name: '', $pseudo-element: before, $non-breaking: false)
```

### Exemple d'usage :

```(sass)
block-agenda
    .top
        a
            @include icon(arrow-right, after, true)
```



