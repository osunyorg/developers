---
title: Cartes
---

Les `organisations` et les `campus` on un affichage en mode carte.

## Composant de carte

Les cartes permettent une visualisation cartographique de lieux, comme les organisations ou les campus. Elles s'appuient sur la librairie open source Leaflet et Open Street Map.

## Accessibilité

Comment intégrer correctement l'usage de la carte via un lecteur d'écran et/ou clavier.

Recommandation d'Aurélien Levy de Temesis :

- Lister hors de la carte, dans une liste `<ul>` les lieux et adresses.
- Masquer aux lecteurs d'écran les pins (markers) des lieux et empêcher le focus et l'utilisation du clavier ouverture/fermeture de la popin associé à chaque pin.
- Ajouter un role `region` sur la carte, avec un `aria-label` précisant la nature du contenu (par exemple: "Localisation des campus") ou un fallback comme "Vue cartographique" si le titre du bloc n'est pas contribué. Ajouter un `aria-describedby` expliquant que la carte est interactive et que l'on peut utiliser les flèches clavier pour naviguer dedans.
- Rendre accessible les fonctionnalités de la carte et traduire les contenus en anglais : zoom / dézoom, crédit Leaflet et Open Street Map.
