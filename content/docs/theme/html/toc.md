---
title: Table des matières
---

## Le besoin

Les documents présentant des hiérarchies longues sont grandement facilités par une table des matières (Table Of Contents, TOC), qui permet :
- de comprendre avant la lecture le plan du document
- d'aller directement à une partie spécifique
- de savoir lors de la lecture où on se situe


Exemples :
- https://opcd.co/fr/veille-scientifique/articles/
- https://www.iut.u-bordeaux-montaigne.fr/formation/offre-de-formation/metiers-du-multimedia-et-de-l-internet/
- https://www.communication-democratie.org/fr/plaidoyers/lutte-contre-le-greenwashing-au-niveau-europeen/

## L'UI

En mobile :
- une barre sticky en haut de page indiquant la position actuelle
- au clic, un offcanvas affichant toute l'arbo et la position actuelle signalée visuellement

En desktop, 2 cas, selon si pleine largeur ou pas.

Desktop pleine largeur :
- une barre sticky en haut de page indiquant la position actuelle
- au clic, un offcanvas affichant toute l'arbo et la position actuelle en gras

Desktop avec sidebar :
- TOC à gauche dans la sidebar
- highlight visuel de la partie en cours

## Le balisage

Le balisage idéal est :
```html
<nav class="toc" aria-label="Table des matières">
  <ol>
    <li>
      <a href="#essential">Essentiel</a>
    </li>
    <li>
      <a href="#presentation">Présentation</a>
    </li>
    <li>
      <a href="#pedagogy">Pédagogie</a>
    </li>
    <li>
      <a href="#results">Après la formation</a>
    </li>
    <li>
      <a href="#admission">Admission</a>
    </li>
  </ol>
</nav>
```