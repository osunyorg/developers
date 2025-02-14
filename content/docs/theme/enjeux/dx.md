---
title: Simple à coder
---

## Raccourcis en développement

`ctrl+g` pour afficher la grille.

`ctrl+w` passer d'une page pleine largeur à une page avec barre latérale, et inversement.


## Données d'exemple

Tester et développer un thème avec des jeux de données par défaut

### Problème

Une solution commune à deux tâches est proposée :
- la création d'un nouveau site (comment travailler sans données ?)
- la mise à jour d'un site existant (comment être sûr de couvrir les cas qui ne sont pas dans les données ?)

### Solution

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

## Intégration continue

Osuny s'appuie sur une chaîne d'intégration continue.
Cet article la décrit, et définit de quelle façon nous avons besoin d'être alertés des problèmes sur cette chaîne.

### Osuny -> référentiels Git

#### Cas 
La mise à jour d'un site est déclenchée par :
- une mise à jour du contenu envoyée par le back-office Osuny.
- une mise à jour du thème envoyée par le back-office Osuny.
- une mise à jour directe du repository (par exemple lorsqu'on travaille sur le site et qu'on fait évoluer le style).

#### Surveillance
Dans tous ces cas le repository git (GitHub / GitLab) est mis à jour et déclenche l'étape 2, la mise à jour des sites Web.
En cas de plantage de cette étape, on est prévenus par la levée d'une exception au niveau de l'application Osuny (remontée dans Bugsnag / Slack), ou par un rejet du push manuel.


### Git -> sites Web

Il y a des process différents selon le type de repository et/ou le type d'hébergement.

#### Netlify
Certains hébergeurs permettent de se brancher directement sur un repository Git pour lancer leur propre workflow interne (par exemple Netlify). Dans ce cas, tout se configure directement au niveau de l'hébergeur.

#### Monitoring Netlify
Côté Netlify, on peut paramétrer une alerte mail lorsqu'un déploiement (compilation incluse) échoue.

#### Workflows spécifiques
Si ce n'est pas le cas, c'est au niveau du repository qu'on va créer un workflow (GitHub Action ou GitLab job) qui sera exécuté à chaque push. Le workflow est découpé en 2 parties : la compilation du site et l'envoi sur l'hébergement. On a besoin d'être prévenu en cas de plantage de l'une ou l'autre de ces parties.

#### Monitoring des workflows
Ce sujet a connnu plusieurs étapes.

Situation en 2023 : GitHub envoie une notification aussi bien lorsque le workflow échoue que lorsqu'il est annulé. Or il est annulé si un job est mis en attente alors que d'autres similaires sont présents. Du coup, en cas de modifications rapprochées de contenu, et donc de push sur le repository Git rapprochés, on a pas mal de tâches annulées, donc pas mal de notifications (inutiles). On a désactivé la notification (au niveau du compte GitHub osuny-bot) pour éviter le spam. Par conséquence, on n'a plus de notifications non plus lorsque le job échoue réellement. On voudrait rétablir les notifications en cas d'échec du job tout en continuant à ignorer les jobs annulés.

Situation en 2024 : Côté GitHub Action, on peut ajouter une étape au workflow, qui se déclenche à la complétion d'un job (quel que soit le statut). On filtre sur le résultat, et s'il s'agit d'un échec ("failure") on notifie une URL de webhook. En l'occurence, on a préparé dans Slack une application qui s'appelle par webhook, branché au canal dans lequel on souhaite faire le reporting. Du coup en cas d'échec, on est bien notifiés dans le canal Slack.

#### Non-régression visuelle
Surveillance des sites webs avec des thèmes changés en profondeur.
Il faudrait que, dans le workflow de mise à jour du site, Percy permette de détecter des changements avant / après.  
On a besoin d'un jeu de données de test, et que sur ces données on puisse déterminer qu'un site n'a pas "sauté" avec la mise à jour du thème.
Ce jeu de données ne doit pas être mis en production.
Il faut probablement (idée Sebouchu) un CI qui instancie une machine, récupère les données Example et la version du thème testé, et lance Percy dessus.
