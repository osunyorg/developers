---
title: Icônes
---

Le mixin icon, déclaré comme ceci : ```@include icon(icon-name, pseudo-element)``` où "icon" correspond au nom de l'icon défini dans le fichier [configuration.sass](/docs/theme/theme-aaa/configuration/#icons) et "pseudo-element", paramétré par défaut comme ```::before``` permet de définir quel pseudo-element contient l'icon. Ce deuxième paramètre est donc facultatif.

```sass
@mixin icon($icon-name: '', $pseudo-element: before, $font-size: px2rem(10))
    &::#{$pseudo-element}
        content: map-get($icons, $icon-name)
        display: inline-block
        font-family: 'Icon'
        font-size: $font-size
        font-style: normal
        font-variant: normal
        font-weight: normal
        line-height: 1
        speak: never
        text-transform: none
        vertical-align: middle
        @content // TODO : important de documenter ça
```

## Overider les icônes Osuny

Il est possible d'écraser les icônes d'Osuny en utilisant la configuration css.

Le code repose sur le tableau `$icons-custom`.

### Example
```sass
$icons-custom-font-family: "my-custom-icon-font-family-name"

$icons-custom: ()
// Override de la flèche pointant la droite avec le code typo de la font icon custom
$icons-custom: map-merge($icons-custom, ("arrow-right-line": "\ea6c"))
```