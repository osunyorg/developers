---
title: Icônes
weight: 10
aliases: 
- /docs/website/coder-le-side/icons/
---

*Comment utiliser des icônes dans son site ?*

## Police d'icônes

Le thème s'appuie sur [Remix Icon](https://remixicon.com/). Vous trouverez les icônes disponibles sur le site d'[exemple osuny](https://example.osuny.org/icones/) et dans le [figma du thème](https://www.figma.com/design/tQh3oLfNwSt8aoVDuUjQt0/Th%C3%A8me-Osuny?node-id=513-1292).

## Utilitaire d'icônes

Vous trouverez les codes des icônes du thème dans `themes/osuny/assets/sass/_theme/configuration/icons.sass`

Pour insérer une icône en `sass`, utilisez le mixin `icon`.

### Mixin `icon`

| Paramètres | Détail | Exemple | Valeur par défaut |
| ---------- | -------------- | ------ | ------- |
| $icon-name | nom de l'icône | `arrow-right-line` |
| $pseudo-element | Ajoute l'icône en `::before` ou `::after` cela permet de choisir si l'icône vient avant ou après l'élément visé | `before` ou `after` | `before` |
| $non-breaking  | Ajoute un espace insécable avant le caractère d'icône | `true` ou `false` | `false` |

```{filename="themes/osuny/assets/sass/_theme/utils/icons.sass"}
@mixin icon($icon-name: '', $pseudo-element: before, $non-breaking: false)
```

### Exemple d'usage :

```(sass)
block-agenda
    .top
        a
            @include icon(arrow-right, after, true)
```

## Customiser les icônes :

Vous pouvez remplacer les icônes existantes en utilisant les variables `sass` :

### Créer la font icon

Vous pouvez utiliser [IcoMoon](https://icomoon.io/) pour la générer.

{{< callout type="warning" >}}
Attention, pour que les icônes respectent la dimensions et les espacements des icônes natives du thème, vous devez importer vos icônes dans un cadre de 24x24px
![Exemple d'icônes en 24x24px sur Figma](custom-icons.png)
{{</ callout >}}

### Importer la font icon

De la même façon que vous [importer vos polices](https://developers.osuny.org/docs/website/coder-le-site/fonts/#charger-les-polices), importer votre font icon : 

```(sass)
@include font-face("MyCustomIconFont", MyCustomIconFont/MyCustomIconFont)
```

### Remplacer des icônes

```(sass)
$icons-custom-font-family: "MyCustomIconFont"
$icons-custom: ()
$icons-custom: map-merge($icons-custom, ("arrow-right-line": "\e005"))
$icons-custom: map-merge($icons-custom, ("arrow-left-line": "\e006"))
```

