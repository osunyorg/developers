---
title: CSS
weight: 1
---

## Organiser ses fichiers

Nous recommandons deux arborescences pour ranger les styles :
- architecture simple
- architecture complète

## Architecture simple

Si votre style n'excède pas les 200 lignes, vous pouvez adopter une version simple.

{{< filetree/container >}}
  {{< filetree/folder name="assets" >}}
    {{< filetree/folder name="sass" state="open" >}}
      {{< filetree/file name="_configuration.sass" >}}
      {{< filetree/file name="_fonts.sass" >}}
      {{< filetree/file name="_style.sass" >}}
      {{< filetree/file name="main.sass" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

`_configuration.sass` : contient les paramètres de sass, son contenu doit se limiter à uniquement les variables `sass`

`_fonts.sass` : contient les font-face uniquement. Voir ["Ajouter des polices"](/docs/website/coder-le-site/fonts/)

`_style.sass` : contient les styles spécifiques. De façon à maintenir le code du site au plus proche du thème, vérifiez bien que les modifications nécessaires au site ne soit pas réalisable via les configurations de style [`_configuration.sass`](https://github.com/osunyorg/theme/blob/main/assets/sass/_theme/_configuration.sass) et du thème [`config.yaml`](https://github.com/osunyorg/theme/blob/main/config.yaml)

`main.sass` : ce fichier est la source du style compilé par le site, il est obligatoire, ne contient que des imports, et doit respecter un ordre précis d'appel :

```sass {filename="assets/sass/main.sass"}
@import "_theme/utils"
@import "_fonts"
@import "_configuration"
@import "_theme/hugo-osuny"
@import "_style"
```


## Architecture complète

Si votre ficher _style.sass dépasse les 200 lignes, séparez les trois fichiers reprenant l'architecture du thème.

{{< filetree/container >}}
  {{< filetree/folder name="assets" >}}
    {{< filetree/folder name="sass" state="open" >}}
      {{< filetree/file name="_configuration.sass" >}}
      {{< filetree/file name="_fonts.sass" >}}
      {{< filetree/file name="_blocks.sass" >}}
      {{< filetree/file name="_design-system.sass" >}}
      {{< filetree/file name="_sections.sass" >}}
      {{< filetree/file name="main.sass" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

`_design-system.sass` : Premier niveau, concerne les éléments du design system.

`_blocks.sass` : Second niveau, concerne les blocs.

`_sections.sass` : Troisième niveau, concerne les spécificités liés aux différentes sections (types de contenu).

```sass {filename="assets/sass/main.sass"}
@import "_theme/utils"
@import "_fonts"
@import "_configuration"
@import "_theme/hugo-osuny"
@import "_utils"
@import "_design-system"
@import "_blocks"
@import "_sections"
```
