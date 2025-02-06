---
title: Auditer un site
weight: 2
aliases:
  - /docs/theme/a11y/change-me/
---

*Réaliser l'audit RGAA d'un site créé avec Osuny, en partant de l'audit socle du thème*

![](/images/home/audit.jpg)

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

