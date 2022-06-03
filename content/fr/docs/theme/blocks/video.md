---
title: "Vidéo"
description: >
  Bloc pour afficher une vidéo en embed
---

## Présentation

Image à insérer


## Data

### JSON (Osuny)

* url ```string```
* transcription ```text```

```json
{
  "url": "https://vimeo.com/679674827",
  "transcription": "Ceci est le texte de transcription de la vidéo qui rend le contenu accessible (RGAA)"
}
```

### Static (Hugo)

* url ```string```
* transcription ```text```

```
- template: video
  title: >-
    Titre du bloc
  position: 1
  data:
    url: >-
      https://vimeo.com/679674827
    transcription: >-
      Ceci est le texte de transcription de la vidéo qui rend le contenu accessible (RGAA)
```

#### Notes

Osuny renvoie l'URL entrée par l'utilisateur, c'est le thème Hugo qui crée le code embed qui permet de modifier le type d'intégration en front.
