---
title: Connexions et dépendances
weight: 3
description: Envoyer tous les fichiers nécessaires
---

Pour être mis en ligne, les sites doivent être envoyés sur un référentiel Git.
Pour cela, il faut lister tous les objets nécessaires et les mettre à jour s'ils ont évolué.
Certains doivent être créés, d'autres modifiés, d'autres supprimés.
Le processus est documenté dans la page [Export Hugo](/docs/admin/communication/sites-web/export/).

La question traitée ici est celle de la liste des objets, et de la mise à jour efficiente :
- comment ajouter les objets à la liste quand c'est nécessaire ?
- comment déclencher des mises à jour les plus précises possibles (ni trop, ce qui causerait du calcul inutile, ni trop peu, ce qui causerait des manques de données) ?
- comment supprimer les objets quand c'est nécessaire ?

## Objet direct
Un objet connecté directement à un site Web, comme une page ou une actualité.
Le site Web lui-même n'est pas strictement un objet direct, mais peut servir de source à un objet indirect, par sa propriété `about`.

## Objet indirect
Un objet connecté à un site Web par le biais d'un objet direct. 
Les objets indirects peuvent se connecter entre eux, et ils héritent de la connexion à l'objet direct.
Un objet indirect peut être connecté à plusieurs objets directs, au sein du même site ou de plusieurs sites.

## Dépendances
L'ensemble des objets liés à un autre et nécessaires à son affichage dans le site.
Par exemple, sont des dépendances d'une actualité :
- son image à la une
- son auteur
- la photo de l'auteur (si elle est affichée sur la page de l'actualité)

## Références
L'ensemble des objets qui font référence à l'objet courant, en utilisant son `path` ou son `slug`.
Par exemple :
- une actualité est référencée par l'ensemble des blocs actualité (même si la réalité est un peu plus subtile)
- une page est référencée par son parent (dans les children)

## Connexions d'un site Web
On appelle "connexions" l'ensemble des liens entre un site Web et des objets indirects qui apparaissent sur ce site.
On distingue connexions et dépendances pour clarifier la réflexion.
Les dépendances mêlent indistinctement objets directs et indirects.
Les connexions sont exclusivement des objets indirects.

À noter, la plupart des connexions concernent des objets ActiveRecord persistés en base de données, mais pas tous.
Les composants et templates de blocs, par exemple, ne sont pas des objets persistés en BDD.

## Événements déclencheurs
Certains événements (création, suppression, lien...) déclenchent certaines mises à jour de contenus.
Pour éviter les calculs inutiles, le lien entre événements déclencheurs et dépendances doit être précis.

