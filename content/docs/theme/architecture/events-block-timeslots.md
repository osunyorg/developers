---
title: Gestion des créneaux dans le bloc agenda
weight: 8
---

## Cas initial

Sur le site du Conservatoire Barbizet, un bloc agenda fait figurer les deux créneaux du même événement à la suite : la première partie d'une journée portes ouvertes est le matin, l'autre l'après-midi. Le bloc agenda n'affichant pas les horaires des événements récurrents, les deux événements côte à côte ressemblent à un bug.

### Solution

Le bloc événements a 3 mode de sélection : `category`, `selection` et `all`.
Pour résoudre le problème d'affichage ci-dessus, seul le mode "sélection" doit exclure les heures, car cela ne prend pas en compte les récurrents (cela affiche le prochain créneau comme un événement global).

## Cas particulier

Dans un événement ayant une date de début et une date de fin, avoir deux créneaux rapprochés, avant la fin de l'événement, créé un affichage avec la mention de la date "Du 28 avril au 30 mai" avec un horaire à 9h. À quelle date se réfère cet horaire ?

### Solution

Dans les modes "catégorie" et "tous", les événements affichés sont listés au même niveau que les créneaux. Cela permet d'identifier chaque créneau indépendamment de la durée totale de l'événement. La date affichée doit donc être celle du créneau.

En revanche, dans le mode "sélection", la date d'un événement récurrent doit être celle complète ("Du 28 avril au 30 mai"), car le créneau affiché se réfère à l'événement global et non à un créneau en particulier (en réalité le prochain).
