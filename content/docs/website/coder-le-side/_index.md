---
title: Coder le site
weight: 3
description: >
  Quand les paramètres ne suffisent plus, on peut facilement coder ce que l'on veut

---

## Modifier le style

### Organiser ses fichiers

Nous recommandons deux arborescences pour ranger les styles.

#### Ranger simplement

Si votre style n'excède pas les 200 lignes, vous pouvez adopter une version simple :

`_configuration.sass` : contient les paramètres de sass, son contenu doit se limiter à uniquement les variables `sass`

`_fonts.sass` : contient les font-face uniquement. Voir ["Ajouter des polices"](/docs/website/coder-le-side/fonts/)

`_style.sass` : contient les styles spécifiques. De façon à maintenir le code du site au plus proche du thème, vérifiez bien que les modifications nécessaires au site ne soit pas réalisable via les configurations de style [`_configuration.sass`](https://github.com/osunyorg/theme/blob/main/assets/sass/_theme/_configuration.sass) et du thème [`config.yaml`](https://github.com/osunyorg/theme/blob/main/config.yaml)


`main.sass` : ce fichier est la source du style compilé par le site, il est obligatoire, ne contient que des imports, et doit respecter un ordre précis d'appel :

```{filename="main.sass"}
@import "_theme/utils"
@import "_fonts"
@import "_configuration"
@import "_theme/hugo-osuny"
@import "_style"
```



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




TODO expliquer comment configurer le css au-delà de la configuration :
- la version très simple à 1 seul fichier
- l'arborescence efficace pour un site plus sophistiqué
- la possibilité de remplacer des fichiers SASS du thème
- la possibilité de remplacer HTML du thème
- comment gérer le javascript


## Raccourcis en développement

`ctrl+g` pour afficher la grille.

`ctrl+w` passer d'une page pleine largeur à une page avec barre latérale, et inversement.

`ctrl+i` permet de contrôler les dimensions des images chargées.

`ctrl+p` si dans config.yaml ```debug.productionUrl``` est spécifié, permet d'ouvrir la page courante en production.
