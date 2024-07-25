---
title: Carousel
---

## Manager
Manager est chargé de l'instanciation de tous les carousels d'une page. 

Il est responsable de transmettre les événements de redimension de la fenêtre à tous les carousels, et détecte quels sont les carousels visibles, et qui ont le focus.s

## Instance

L'instance de carousel représente 1 carousel dans la page.
Elle est en charge des événements et agit comme un chef d'orchestre


Elle est composée de :
- 1 `Slider` qui contient les images
- 1 `Pagination` qui gère la navigation
- 1 `Autoplayer` qui gère la lecture automatique

## Autoplayer
L'autoplayer se charge de passer automatiquement le carousel au prochain slide, à un intervalle donné.

Il est contrôlé par 4 fonctions qui permettent de : 
- mettre en pause `pause()`, reprendre `unpause()` l'autoplay,
- arrêter `stop()`, Démarrer `start()` l'autoplay.

Il met également à jour la progression de l'UI dans la pagination dans le cas où celle-ci est active.

## Slider
Slider est l'ensemble des slides qui se déplacent horizontalement. 
Il est chargé du calcul de translations en fonction de l'index de slide visé.
Il est composé d'un tableau de `Slide`.

## Pagination
Système de contrôle du carousel.

Elle est composée de:
- `PaginationButton` qui représente chaque slide, ainsi que sa progression dans le cas de l'autoplay. Au click le bouton amène vers le slide correspondant.
- `ToggleButton` qui représente l'état de l'autoplayer ( Démarré ou arrêté), et au clique déclenche le démarrage `start()` ou l'arrêt `stop()` de l'autoplayer. 

