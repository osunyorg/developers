---
title: "Vidéo"
description: >
  Bloc pour afficher un embed
---

## Edit

* titrle ```string```
* url ```url```
* transcription ```textarea```

## Static

Osuny renvoie l'url entrée par l'utilisateur, c'est le thème Hugo qui crée le code embed qui permet de modifier le type d'intégration en front.

```
- template: video
  title: >-
    Titre du bloc
  url: >-
    https://vimeo.com/679674827
  transcription: >-
    Ceci est le texte de transcription de la vidéo qui rend le contenu accessible (RGAA)
```
