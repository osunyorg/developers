---
title: Taille des images
---

## Problématique

Le fichier de configuration de taille des images est en bazar.

## Cas concret

Pour modifier la taille des images d'un bloc projet en mise en forme liste il faut :

Dans le fichier `hugo.yaml` il y a deux clés :
`image_sizes.blocks.projects.alternate` ou `image_sizes.sections.projects.alternate`.

Comment savoir laquelle est la bonne ?

## Situation idéale

Pouvoir modifier simplement la taille d'une image en fonction de son type et sa mise en forme.

## Solution proposée


Un fichier bien rangé avec une configuration par défaut. 

```
image_sizes:
  components:
    lightbox: …
  default:
    hero:
      single: …
      index: …
    layouts:
      alternate:
        mobile:   360
        tablet:   555
        desktop:  520
      cards:
        mobile:   360
        tablet:   555
        desktop:  575
      carousel:
        mobile:   360
        tablet:   555
        desktop:  575
      grid:
        mobile:   360
        tablet:   555
        desktop:  575
      large: 
        mobile:   400
        tablet:   555
        desktop:  894
      list:
        mobile:   360
        tablet:   555
        desktop:  374
  projects:
    hero: 
      single: …
      index: …
    layouts:
      alternate: …
```

Un système de fallback pour utiliser la valeur par défaut : ça permet de nettoyer le fichier de config et de pouvoir modifier une taille d'image par type `section` et mise en forme `layout`.
