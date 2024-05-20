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

#### Ranger simplement

Si votre ficher _style.sass dépasse les 200 lignes, séparez le trois fichiers reprenant l'architecture du thème.

`_design-system.sass` : Premier niveau, concerne les éléments du design system.

`_blocks.sass` : Second niveau, concerne les blocs.

`_sections.sass` : Troisième niveau, concerne les spécificités liés aux différentes sections (types de contenu).

```{filename="main.sass"}
@import "_theme/utils"
@import "_fonts"
@import "_configuration"
@import "_theme/hugo-osuny"
@import "_utils"
@import "_design-system"
@import "_blocks"
@import "_sections"
```

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

## Modifier le javascript

Vous pouvez ajouter du javascript en ajoutant vos fichiers dans `assets/js`. Ensuite créer un fichier `assets/js/main.js` de façon à remplacer/overrider celui du thème : n'oubliez pas d'importer les fichiers javascript du thème à l'intérieur en plus des votres de façon à maintenir le bon fonctionnement du site : 

```{filename="/assets/js/main.js"}
// Librairies
import './vendors/lightbox';
// Fichiers du thème nécessaire à son bon fonctionnement
import './theme/';
// Customisation
import './custom-js-1.js';
import './custom-js-2.js';
```

## Modifier les partiels HTML

Vous pouvez remplacer n'importe quels fichiers du thème grace à Hugo. Quand le site compile, Hugo va cherche le fichier à la racine avant celui du thème. Pas exemple si vous souhaitez modifier l'en-tête (`hero`) des actualités, vous pouvez créer le fichier

```
/layouts/partials/posts/hero-single.html
```

...qui viendra remplacer/overrider le fichier du thème.

```
themes/osuny-hugo-theme-aaa/layouts/partials/posts/hero-single.html
```

{{< callout type="warning" >}}
  Attention, en décidant d'écraser un fichier HTML, vous ne bénéficierez plus des mises à jour de ce fichier. Il faudra alors mettre à jour manuellement le fichier si le fichier original du thème subit des modifications.
{{< /callout >}}


## Outils

### Utilitaires

De nombreux mixins permettent de simplifier l'intégration et la customisation du site :

- [Les grilles](/docs/website/coder-le-side/grilles/)
- [Les icônes](/docs/website/coder-le-side/icons/)

### Raccourcis de développement

`ctrl+g` pour afficher la grille.

`ctrl+w` passer d'une page pleine largeur à une page avec barre latérale, et inversement.

`ctrl+i` permet de contrôler les dimensions des images chargées.

`ctrl+p` si dans config.yaml ```debug.productionUrl``` est spécifié, permet d'ouvrir la page courante en production.
