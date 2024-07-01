---
title: Multilingue
weight: 3
---

## Langues de l'université

Actuellement on choisit explicitement les langues disponibles dans une université parmi l'ensemble des langues disponibles sur Osuny (model `Language`). 
Ensuite on choisit une langue par défaut pour l'université (nécessairement parmi les langues disponibles pour cette université).
Par la suite tous les objets indirects pourront être traduits dans toutes les langues disponibles pour l'université.  

## Langues du site Web

A la création d'un site web on choisit une liste de langues disponibles pour le site Web en fonction des langues disponibles pour l'université. 
Et on choisit la langue par défaut du site parmi les langues sélectionnées (on force le choix d'au moins une langue pour pouvoir avoir cette langue par défaut).

## Changement de langue

Un choix de langue dans le menu permet de naviguer entre les langues de l'Université, ou entre les langues du site Web (qui peut être un ensemble plus réduit).

### Exemple

- Université en 3 langues (anglais, français, italien)
- Site en 2 langues (anglais, français)

Dans ce cas, le choix de langue affichera 3 choix si l'on est sur le tableau de bord, 2 choix si l'on entre dans le site.


Si l'on navigue en italien et qu'on va sur le site, il pourrait y avoir 2 logiques :
- proposer de créer le site en italien
- rediriger vers l'anglais ou le français

