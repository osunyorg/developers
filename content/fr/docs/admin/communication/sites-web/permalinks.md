---
title: Permaliens
weight: 2
description: >
  Gérer les urls dans les sites générés avec Hugo
---

## Contexte

Les objets composant les sites (pages, posts, persons...) ont une URL, qui est supposée être permanente : le permalien. 

Ce permalien, dans Osuny, ne contient pas le domaine ni le protocole, mais juste le path, la partie après le nom de domaine. Il est composé de plusieurs parties, par exemple :

```
/fr/actualites/2022-10-22-un-article
```
soit 
```
/:localisation/:section/:slug
```

Ce motif de permalien est l'un des motifs possibles, parmi de nombreux autres.

Malheureusement, ces permaliens pouvent bouger pour plusieurs raisons : 
- activation ou désactivation d'une langue
- changement de slug (suite à changement de date ou de titre)
- changement de slug de section
- changement technique dans le site

À chaque changement, si rien n'est fait techniquement, la précédente URL devient une erreur 404. Il faut éviter cela et créer une redirection 301 (permanente) afin de renvoyer la personne sur le bon permalien.

Pour parvenir à ce résultat, il faut conserver la trace des anciens permaliens. Quand on dispose de cette information, on peut agir de 2 façons :
- en donnant à Hugo des alias, ce qui lui permet de faire des redirections vers les nouvelles pages
- en exportant une table de redirection, dont le format dépend du serveur

