---
title: Intégration continue
weight: 9
description: >
  Du back office Osuny au site web
---

## Étape 1 : mise à jour du repository git

La mise à jour d'un site est déclenchée par :
- une mise à jour du contenu envoyée par le back-office Osuny.
- une mise à jour du thème envoyée par le back-office Osuny.
- une mise à jour directe du repository (par exemple lorsqu'on travaille sur le site et qu'on fait évoluer le style).

Dans tous ces cas le repository git (GitHub / GitLab) est mis à jour et déclenche l'étape 2.
En cas de plantage de cette étape, on est prévenus par la levée d'une exception au niveau de l'application Osuny (remontée dans Bugsnag / Slack), ou par un rejet du push manuel.


## Étape 2 : la compilation et la mise en ligne du site.

Il y a des process différents selon le type de repository et/ou le type d'hébergement.

Certains hébergeurs permettent de se brancher directement sur un repository Git pour lancer leur propre workflow interne (par exemple Netlify). Dans ce cas, tout se configure directement au niveau de l'hébergeur.

Si ce n'est pas le cas, c'est au niveau du repository qu'on va créer un workflow (GitHub Action ou GitLab job) qui sera exécuté à chaque push. Le workflow est découpé en 2 parties : la compilation du site et l'envoi sur l'hébergement. On a besoin d'être prévenu en cas de plantage de l'une ou l'autre de ces parties.

Situation initiale : GitHub envoie une notifiaction aussi bien lorsque le workflow échoue que lorsqu'il est annulé. Or il est annulé si un job est mis en attente alors que d'autres similaires sont présents. Du coup, en cas de modifications rapprochées de contenu, et donc de push sur le repository Git rapprochés, on a pas mal de tâches annulées, donc pas mal de notifications (inutiles). On a désactivé la notification (au niveau du compte GitHub osuny-bot) pour éviter le spam. Par conséquence, on n'a plus de notifications non plus lorsque le job échoue réellement.

On voudrait rétablir les notifications en cas d'échec du job tout en continuant à ignorer les jobs annulés.

### Solution

Côté Netlify, on peut paramétrer une alerte mail lorsqu'un déploiement (compilation incluse) échoue.

TODO : à tester lorsqu'un déploiement est annulé.

Côté GitHub Action, on peut ajoute une étape au workflow, qui se déclenche à la complétion d'un job (quel que soit le statut). On filtre sur le résultat, et s'il s'agit d'un échec ("failure") on notifie une URL de webhook. En l'occurence, on a préparé dans Slack une application qui s'appelle par webhook, branché au canal dans lequel on souhaite faire le reporting. Du coup en cas d'échec, on est bien notifiés dans le canal Slack.
