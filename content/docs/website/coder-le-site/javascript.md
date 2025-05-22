---
title: JavaScript
weight: 3
aliases: 
- /docs/website/coder-le-side/javascript/
---

Vous pouvez ajouter du javascript en ajoutant vos fichiers dans `assets/js`. 
Ensuite créer un fichier `assets/js/main.js` de façon à remplacer/overrider celui du thème : n'oubliez pas d'importer les fichiers javascript du thème à l'intérieur en plus des votres de façon à maintenir le bon fonctionnement du site : 

```javascript {filename="assets/js/main.js"}
// Librairies
import './vendors/lightbox';
// Fichiers du thème nécessaire à son bon fonctionnement
import './theme/';
// Customisation
import './custom-js-1.js';
import './custom-js-2.js';
```