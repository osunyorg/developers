---
title: Workflows
weight: 5
---

#### Monitoring des workflows
Ce sujet a connnu plusieurs étapes.

Situation en 2023 : GitHub envoie une notification aussi bien lorsque le workflow échoue que lorsqu'il est annulé. Or il est annulé si un job est mis en attente alors que d'autres similaires sont présents. Du coup, en cas de modifications rapprochées de contenu, et donc de push sur le repository Git rapprochés, on a pas mal de tâches annulées, donc pas mal de notifications (inutiles). On a désactivé la notification (au niveau du compte GitHub osuny-bot) pour éviter le spam. Par conséquence, on n'a plus de notifications non plus lorsque le job échoue réellement. On voudrait rétablir les notifications en cas d'échec du job tout en continuant à ignorer les jobs annulés.

Situation en 2024 : Côté GitHub Action, on peut ajouter une étape au workflow, qui se déclenche à la complétion d'un job (quel que soit le statut). On filtre sur le résultat, et s'il s'agit d'un échec ("failure") on notifie une URL de webhook. En l'occurence, on a préparé dans Slack une application qui s'appelle par webhook, branché au canal dans lequel on souhaite faire le reporting. Du coup en cas d'échec, on est bien notifiés dans le canal Slack.
