---
title: "Tester des jeux de données"
description: >
  Comment tester et développer un thème avec des jeux de données par défaut
---

# Constat

Une solution commune à deux tâches est proposée :

- La création d'un nouveau site
- La mise à jour d'un site existant

# Proposition

Ajouter en submodule git un site d'exemples : https://github.com/noesya/osuny-example

Dans ce site d'exemple, une configuration hugo a été ajoutée dans config/examples/config.yaml :

```
contentDir: "osuny-example/content"
dataDir: "osuny-example/data"
```

Cela permet ensuite via une commande, de lancer notre site en s'appuyant sur les données de osuny-example, données présentes dans osuny-example/content et  osuny-example/data.

Ajouter une commande à lancer via yarn (ou npm) qui permet d'ajouter le submodule, le mettre à jour, puis lancer le site en s'appuyant sur la configuration du site d'exemple (osuny-example).

Commandes séparées dans le package.json d'un site :

```json
"scripts": {
  "update": "git pull --recurse-submodules --depth 1 && git submodule update --remote",
  "setup-example": "git submodule add https://github.com/noesya/osuny-example",
  "server-example": "hugo server --config 'osuny-example/config/example/config.yaml'",
}
```

Commande chaînée :

```json
"scripts": {
  "example": "yarn setup-example > /dev/null || yarn update && yarn server-example"
}
```





