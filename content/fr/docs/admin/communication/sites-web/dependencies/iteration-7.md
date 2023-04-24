---
title: Itération 7
description: Travail semaine du 24 avril 2023
---

## La gestion de l'about du website

On rajoute un process au changement de l'about d'un website pour le connecter s'il s'agit d'un objet indirect.

Par exemple, si on déclare qu'un site est un site d'école, on crée une connexion de cette école au site avec le site comme source directe.

En cascade, on connecte les objets indirects listés comme dépendances de l'about, par exemple, pour une école, on va déclarer des connexions aux formations et aux diplômes.

Lors de ce process, on appelle la méthode qui nettoie les connexions obsolètes afin de supprimer les connexions d'un potentiel ancien about.

## La (non-)gestion des menus

Pour un site indépendant, en dehors des titres intermédiaires et des URLs externes, on ne peut créer des éléments de menu que pour des objets directs (actualités et pages).

En déclarant un about au site, par exemple une école, on crée des connexions aux dépendances de cet about (voir chapitre « La gestion de l'about du website »).

On peut désormais créer des éléments de menu liés à des formations mais celles-ci sont déjà connectés via le site directement par les dépendances de l'about.

Il est donc inutile de gérer les connexions à ce niveau-là car
- l'about du website ne peut pas être supprimé du website sans être supprimé au global ou déconnecté du website en changeant l'about.
- et par conséquent, la connexion sera toujours présente tant que l'about existe.
