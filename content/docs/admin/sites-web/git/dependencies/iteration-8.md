---
title: Itération 8
description: Réflexion 13 juillet 2023
weight: 8
---

## Durée de synchronisation

Depuis l'ajout des publications HAL, la durée des synchronisations est anormalement longue. En analysant avec un outil de monitoring, on se rend compte que le rendu des fichiers statiques est assez long, ce qui pose la question, lors de la sync d'un site, de retravailler la logique permettant de définir quels objets doivent être réellement mis à jour sur Git.

Actuellement on fait le rendu du statique à chaque fois pour en obtenir un SHA à comparer avec celui du référentiel Git pour définir si oui ou non, la mise à jour est nécessaire.

Il faudrait pouvoir définir, en fonction de la modification d'un objet ou de ses dépendances, qu'un GitFile est invalidé et qu'il aura besoin d'être regénéré à la prochaine synchronisation.

## Problème d'empilement des tâches identiques

Actuellement, on ne vérifie pas qu'un objet est déjà en cours de synchronisation avant de le mettre dans la pile. Pour éviter de se retrouver avec 20 tâches pour le même objet, on peut vérifier la présence d'une tâche en cours en amont.
