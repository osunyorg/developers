---
title: "v8"
weight: 10
---

## Liste des modifications

### Configuration de la taille des images

Rangement et homogénéisation des configurations de la taille des images : https://developers.osuny.org/docs/theme/architecture/taille-des-images/

### Balisage `<ul>` et `<ol>` pour toutes les listes

> [!TIP]
> Amélioration d'accessibilité.

Remplacement des balises `<div>` par des listes html dans les blocs de liste.

> [!WARNING]
> Certains sélecteurs CSS ont été modifiés pour conserver le style.

### Focus-trap dans la table des matières

> [!TIP]
> Amélioration d'accessibilité.

Ajout d'un focus trap pour la table des matières.

https://github.com/osunyorg/theme/issues/1015


### Harmonisation des meta dans les blocs de liste

L'ordre d'affichage des catégories dans une liste est désormais toujours le même.

Les catégories se trouvent sous le résumé pour toutes les natures éditoriales.

{{< cards >}}
  {{< card link="/" title="Actualité" image="image-1.png" >}}
  {{< card link="/" title="Événements" image="image-2.png" >}}
  {{< card link="/" title="Projet" image="image-3.png" >}}
  {{< card link="/" title="Offre d'emploi" image="image-4.png" >}}

{{< /cards >}}

https://github.com/osunyorg/theme/pull/1184

### Espacement verticaux

Pour simplifier et rendre plus robuste les espacements verticaux, on favorisera toujours l'usage d'une margin-top. Le `hero` ne pousse plus vers le bas, mais ce sont les contenus en dessous qui pousse vers le haut.

Cela permet de modifier le comportement en fonction de l'éléments suivant :

```
.hero + .document-content .block-class-specific-class
  margin-top: var(--grid-gutter)
```

### Contenus liés à une formation

Ajout des expositions


### Factorisation CSS des mises en forme

Application des mixins

```
# Mixins

layout-alternate
layout-cards
layout-carousel
layout-large
layout-list
layout-grid
```

https://developers.osuny.org/docs/theme/css/mise-en-page/

### Fédération des actualités


### Lazy loading d'image dans un bouton

Correction du chargement progressif des images dans les boutons d'ouverture de lightbox.


