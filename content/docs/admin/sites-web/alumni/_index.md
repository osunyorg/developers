---
title: Alumni public
weight: 6
---

Cette nouvelle fonctionnalité est codée pour les Beaux Arts de Marseille.

## Architecture

Plusieurs architectures possibles :
1. Extranet en mode public
2. Nouvelle fonctionnalité au niveau du site Web
3. Type particulier de site Web
4. Sous-partie spécifique du site des Beaux-Arts

### Extranet en mode public

Avantages :
- ça permet de faire ce que l'on veut en fonctionnel

Inconvénients :
- charge serveur
- maintenance CSS
- nouvelle porte d'entrée "publique" dans Osuny

### Nouvelle fonctionnalité au niveau du site Web

Fonctionnalité uniquement activable pour les sites d'écoles et de formations, s'il y a des alumni.


Avantages :
- se greffe sur n'importe quel site
- maintenance CSS facile
- les objets des backlinks des personnes sont directement dans le site

Inconvénients :
- pb pour gérer la home et les menus spécifiques en automatique

### Type particulier de site Web

Avantages :
- permet de gérer spécifiquement certains fonctionnalités comme le menu ou la page d'accueil
- maintenance CSS facile

Inconvénients :
- les objets des backlinks des personnes viennent d'un autre site (question de fédération de contenu)

### Sous-partie spécifique du site des Beaux-Arts

C'est une sous-option de l'option 2.


Avantages :
- Simple pour les backlinks

Inconvénients :
- URLs confuses
- 2 menu et 2 CSS en fonction

### Choix : Type particulier de site Web

Il n'y a pas d'inconvénient !

## Implémentation

### Nouveau type

En plus de pouvoir déclarer un site "Site d'école", ou "Site de laboratoire", on peut déclarer un site comme étant "Site d'alumni d'école" ou "Site d'alumni de formation". Il faut dépendre d'un attribut en + de l'about_type.

### Dépendance des alumnis

A déclarer via les dépendances de formation, via les promotions. N'était pas en place car jusque là spécifique aux extranets.

### Export des personnes

Git files pour les personnes. Pas d'url spécifique, url normale de personne.

### Gestion du menu

Lister les promos et les formations (parcours, mentions, options...).

### Gestion de la home

- lister n alumni
- remonter les métriques (nombre d'alumni depuis, nombre de nationalités via taxonomie)

### Backlinks fédérés

Prendre les backlinks d'une personne à partir de contenus fédérés d'un autre site.
