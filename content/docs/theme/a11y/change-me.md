---
title: "Réaliser un audit RGAA"
linkTitle: "Audit RGAA"
weight: 100
description: >-
     Outils et conseils pour réaliser un audit RGAA
---

## Snippets pour faciliter l'audit

### 10 : Présentation de l'information

#### 10.12

> Dans chaque page web, les propriétés d’espacement du texte peuvent-elles être redéfinies par l’utilisateur sans perte de contenu ou de fonctionnalité (hors cas particuliers) ?

Pour faciliter ce test, exécutez ce code directement dans la console js du navigateur :

```
var style = document.createElement('style');
style.innerHTML =
  `
  * { 
    line-height: 1.5em !important;
    word-spacing: 0.16em !important;
    letter-spacing: 0.12em !important;
  }
  p {
    margin-bottom: 2em !important;
  }
  `;
document.body.append(style);
```

