---
title: "Icons"
description: >
  Gestion des icônes
---

### Les icônes sont gérées dans 3 fichiers : 

* `base/_font-icons.sass` : On déclare ici la font-face et le chemin du fichier .woff et .woff2
* `abstracts/_icons.sass` : On déclare ici les jeux clé/valeur des icônes
* `components/_fonts.sass` : On déclare ici les classes associées à partir du tableau clé/valeur définit dans `abstracts/_icons.sass` **(ce fichier ne doit pas être overridé)**

### Ajout d'icônes dans un site 

Pour ne pas perdre la maintenabilité des icons du thème, il faut ajouter une font icons spécifique au site
