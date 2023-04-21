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
- terminer les tests de connexions pour couvrir tous les cas importants
- vérifier les ajouts / suppressions sur Git
- améliorer l'algo de nettoyage (async ?)

