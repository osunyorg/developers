---
title: Composants Javascript
weight: 6
---

Le thème Osuny intègre quelques composants essentiels, développés en ES5 vanille afin de maximiser la rétrocompatibilité avec les anciens navigateurs, d'être légers, aussi simples que possible et parfaitement accessibles.

{{< cards >}}
  {{< card link="carousel" title="Carousel" >}}
  {{< card link="lightbox" title="Visionneuse" >}}
  {{< card link="notes" title="Notes" >}}
{{< /cards >}}

Deux classes sont communes à ces différents composants: 

## Events
Inclut les écouteurs au niveau de la page, de manière à gérer les cas de focus et d'événements en fonction des états des différents composants. Par exemple, si la lightbox est ouverte, l'événement ArrowRight d'entrée clavier est redirigé vers la lightbox ; dans le cas inverse, il est redirigé vers le carrousel d'images.

## Utils
Comprend des méthodes permettant de trouver un élément dans la page ou de transmettre des événements.
