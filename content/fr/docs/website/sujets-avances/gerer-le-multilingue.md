---
title: "Gérer le multilingue"
description: >
  Ce qu'il faut savoir pour les sites utilisant plusieurs langues
---

## Logo différent en fonction de la langue
 
Pour pouvoir modifier les logos en fonction de la langue, il faut créer un fichier dans config/_default avec le code langue en suffix (exemple pour un site en anglais : `config.en.yaml`), et à l'intérieur overrider les paramètres voulus. Par contre j'ai l'impression qu'hugo ne fait pas de deep merge, pour le param logo, il faut donc obligatoirement donner un logo pour le header et un pour le footer (même si c'est le même que celui par défaut).

/config/_default/config.en.yaml
```yaml
params:
  logo:
    header: "/assets/images/logo-en.svg"
    footer: "/assets/images/nations-unies.svg"
```