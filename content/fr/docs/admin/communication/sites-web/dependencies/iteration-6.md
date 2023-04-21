---
title: Itération 6
description: Synthèse au 21 avril 2023
---

Les dépendances fonctionnent, sont (légèrement) testées, et permettent de lister tous les objets nécessaires à l'affiche sur un site Web.
Elles ne tombent pas dans les boucles infinies.

Les connexions se créent correctement, et permettent de savoir dans quels sites Web et dans quel objet direct un objet indirect est utilisé.
Elles se suppriment de façon sous-optimale : en passant par le site Web, avec un algo qui charge tous les objets un par un. 
Cet algo est synchrone.
Les connexions commencent à être testées. Il manque des cas importants.

Il reste à :
- vérifier les ajouts / suppressions sur Git
- terminer les tests de connexions pour couvrir tous les cas importants
- améliorer l'algo de nettoyage (async ?)

## Workflow Git

Les objets indirects déclenchent `save_and_sync` des objets directs par lesquels ils sont connectés.

Les objets directs déclenchent les méthodes `save_and_sync`, `update_and_sync`, `destroy_and_sync` dans les controllers.

La synchronisation vers git se fait dans le concern `WithGit`, utilisé par les objets directs, qui définit ces 3 méthodes.

2 méthodes clés : 
- `sync_with_git`, qui ne gère pas les suppressions
- `destroy_from_git`, qui gère uniquement la suppression de l'objet direct 

Ces 2 méthodes vont gérer les modifications, mais pas les suppressions, déconnexions ou dépublications d'objets indirects.
Ca gère les choses en plus, pas les choses en moins.
Pour les suppressions d'objets indirects, il faut passer par la méthode `destroy_obsolete_git_files` du trait `Communication::Website::WithGitRepository`.
Cette méthode est coûteuse, il faut donc éviter de l'appeler.

La logique générale est celle du delta de dépendances.
S'il manque des dépendances entre avant et après l'enregistrement d'un objet (direct ou indirect), il faut déclencher.
Dit autrement, si certaines dépendances "d'avant" ne se retrouvent pas "après" la sauvegarde, il faut déclencher.