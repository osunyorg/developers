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
components:
  lightbox: …
image_sizes:
  _default:
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
  jobs:
    hero: 
    layouts:
      alternate: …
```

Un système de fallback pour utiliser la valeur par défaut : ça permet de nettoyer le fichier de config et de pouvoir modifier une taille d'image par type `section` et mise en forme `layout`.


### Migration

## Ajout de la clé `components`

Modification de la clé `design-system` pour `components`.

| Ancienne clé                                                    | Nouvelle clé                                            | Type de changement | Commentaire / Action                                                          |
| --------------------------------------------------------------- | ------------------------------------------------------- | ------------------ | ----------------------------------------------------------------------------- |
| `image_sizes.design_system.lightbox.disabled`            | `components.lightbox.disabled`                   | move               | La configuration du lightbox a été déplacée dans `components`.                |
| `image_sizes.design_system.lightbox.in_gallery.disabled` | `components.lightbox.in_gallery.disabled`        | move               | Déplacement pour cohérence avec `components`.                                 |

## Ajout de la clé `_default`

Modification de la clé `design-system` pour `_default`.

| Ancienne clé                                                    | Nouvelle clé                                            | Type de changement | Commentaire / Action                                                          |
| `image_sizes.design_system.hero`                         | `image_sizes._default.hero`                      | move               | Déplacement dans `_default` pour uniformiser les tailles par défaut.          |
| `image_sizes.design_system.layouts`                         | `image_sizes._default.layouts`                      | move               | Déplacement dans `_default` pour uniformiser les tailles par défaut.          |

## `section` et `layouts`

Rangement des sections dans `section` et pas dans `blocks` et ajout de la clé `layouts` pour grouper les mises en forme.

| Ancienne clé                                                    | Nouvelle clé                                            | Type de changement | Commentaire / Action                                                          |
| `image_sizes.blocks.jobs.alternate`                      | `image_sizes.sections.jobs.layouts.alternate`      | move               | Les variantes de layout des blocks sont maintenant regroupées sous `layouts`. |
| `image_sizes.blocks.jobs.grid`                           | `image_sizes.sections.jobs.layouts.grid`           | move               | Idem pour grid.                                                               |
| `image_sizes.blocks.jobs.large`                          | `image_sizes.sections.jobs.layouts.large`          | move               | Idem pour large.                                                              |
| `image_sizes.blocks.jobs.list`                           | `image_sizes.sections.jobs.layouts.list`           | move               | Idem pour list.                                                               |
| `image_sizes.sections.*.large`                           | `image_sizes.sections.*.layouts.large`           | move               | Tous les layouts sectionnels sont regroupés sous `layouts`.                   |
| `image_sizes.sections.*.list`                            | `image_sizes.sections.*.layouts.list`            | move               | Même logique que pour `large`.                                                |


## Cas particulier de l'agenda

Création de la clé `children_in_agenda` pour la taille des images des événements enfants dans le layout agenda.

| Ancienne clé                                                    | Nouvelle clé                                            | Type de changement | Commentaire / Action                                                          |
| `image_sizes.sections.events.parent`                     | `image_sizes.sections.events.layouts.parent`     | move               | La taille “parent” des events est maintenant dans layouts.                    |
| `image_sizes.sections.events.agenda`                     | `image_sizes.sections.events.layouts.agenda`     | move               | Idem pour agenda.                                                             |
| `image_sizes.sections.events.children_in_agenda`         | `image_sizes.sections.events.children_in_agenda` | new                | Nouvelle clé pour gérer la largeur des enfants dans l’agenda.                 |

## Legacy

| Ancienne clé                                                    | Nouvelle clé                                            | Type de changement | Commentaire / Action                                                          |
| `image_sizes.blocks.organization_chart`                  | removed                                                 | remove             | Clé legacy supprimée.                                                         |
| `image_sizes.blocks.partners`                            | removed                                                 | remove             | Clé legacy supprimée.                                                         |


