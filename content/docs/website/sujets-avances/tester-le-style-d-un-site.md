---
title: "Tester le style d'un site"
description: >
  De petits sujets qui peuvent être problématiques
weight: 3
---

## Cloner le site "example" (branche ```site-style```)

https://github.com/osunyorg/example/tree/site-style

## Si vous souhaitez déployer

Créer une autre branche à partir de ```site-style```


## Ajouter le répertoire du site à tester

```
cd themes
git submodule add https://github.com/osunyorg/rebootcommunication
```

## Configurer les themes

Dans ```/config/_default/config.yaml```

```
theme:
  - rebootcommunication
```
