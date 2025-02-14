---
title: Niveaux de titres
---

## Commun

```sass
body
  header
  main
    .hero
      hgroup // optionnel
        h1
    .document-content
      .blocks
        .block-n
          h2
          articles
            h3
  footer
```

## Formations

Visuellement, les blocks font partis du H2 de la section "Présentation", mais il n'y pas de certitude que cette structure ne bouge, on reste donc sur un balisage similaire pour les blocs (comme Commun).

```sass
body
  header
  main
    .hero
      hgroup # optionnel
        h1
    .document-content
      section
        h2
        .blocks
          .block-n
            h2
            articles
              h3
  footer
```
