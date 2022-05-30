---
title: "Intégration HTML (embed)"
description: >
  Bloc pour afficher une intégration HTML (embed)
---

## Présentation

Image à insérer


## Data

### JSON (Osuny)

* code ```textarea```
* transcription ```textarea```

```json
{
  "code": "<iframe title=\"Inline Frame Example\" width=\"300\" height=\"200\" src=\"https://www.openstreetmap.org/export/embed.html?bbox=-0.004017949104309083%2C51.47612752641776%2C0.00030577182769775396%2C51.478569861898606&layer=mapnik\"></iframe>",
  "transcription": "Ceci est le texte de transcription du bloc de code qui rend le contenu accessible (RGAA)"
}
```

### Static (Hugo)

* code ```richtext```
* text ```text```

```
- template: embed
  title: >
    Code d'intégration HTML
  position: 1
  data:
    code: >-
      <iframe title="Inline Frame Example" width="300" height="200" src="https://www.openstreetmap.org/export/embed.html?bbox=-0.004017949104309083%2C51.47612752641776%2C0.00030577182769775396%2C51.478569861898606&amp;layer=mapnik"></iframe>
    transcription: >-
      Ceci est le texte de transcription du bloc de code qui rend le contenu accessible (RGAA)
```
