---
title: Non regression
weight: 5
---

## Amélioration pour le contrôle de non regression

### Analyse automatique des surcouches 

#### HTML

Lancer une analyse des sites contenants des surcouches qui pourraient être impacté par la release : 
Des fichiers modifiés dans la release sont écrasés par une surcouche
Des fichiers modifiés sont appelés (partial) dans une surcouche

#### Style

Trouver un système de suivi qui pourrait remonter les sélecteurs CSS modifiés ?
Actuellement, s’il y a une modification d’un sélecteur, on recherche manuellement dans l’organisation github (https://github.com/osunyorg) si un site ou un thème utilise un sélecteur qui deviendrait obsolète.

### Non régression visuelle

Le système de non régression visuelle compare les pages de la version précédente à la version de la release. Ils remontent les différences dans un canal slack dédié aux sites suivis (par exemple l’équipe front Ecedi veut suivre uniquement les sites de Rennes).


#### Objectifs de contrôle

- Responsivité dans tous les contextes
- Compatibilité navigateurs
- Régression visuelle suite à évolution du thème

https://fr.wikipedia.org/wiki/Test_de_Jo%C3%ABl

##### 1. Responsivité

Écrire une liste des tailles ?

Très grands écrans ?

##### 2. Compatibilité navigateurs

Écrire une liste des contextes ?

Utiliser cross-browser testing ?

Utiliser BackstopJS ?

##### 3. Non régression visuelle

- Github action

- Pre-release du thème

- Dans l’action de pre-release, screenshot d’une série de pages définies en amont 

- Screenshot avec la release précédente

- Mise à jour du thème

- Screenshot post-release

- Envoi des paires de screenshots à un service tiers 

- Service tiers

- App Rails à fabriquer

- A la réception de paires de screenshots : 

- Analyse des diffs d’image

- Si diff, notif Slack et Issue Github

Dans la notif, lien vers une page listant les diffs suite à la release
https://github.com/instantink/image_compare ?