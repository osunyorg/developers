---
title: "Headings"
linkTitle: "Headings"
weight: 100
description: >-
     Architecture des niveaux de titre dans les pages
---

## Commun

```
body
  header
  main
    .hero
      hgroup # optionnel
        h1
    .document-content
      .blocks
        .block-n
          h2
          articles
            h3
  footer
```

## Program


```
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
