---
title: Permaliens
weight: 4
---

Gérer les urls dans les sites générés avec Hugo

## Contexte

Les objets composant les sites (pages, posts, persons...) ont une URL, qui est supposée être permanente : le permalien.

Ce permalien, dans Osuny, ne contient pas le domaine ni le protocole, mais juste le path, la partie après le nom de domaine. 
Il est composé de plusieurs parties, typiquement :

```
/fr/actualites/2022-10-22-un-article/
```
soit
```
/:localisation/:section/:slug/
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

## Inventaire

Les différentes formes utilisées sont :

| Forme | Nom | Description |
| - | - | - |
| 2022-10-22-un-article | slug | Identifiant d'un objet, sans contexte. Pour les catégories ce slug tient compte de la parentèle (categorie-sous-categorie) |
| /2022-10-22-un-article/ | ? | ? |
| /actualites/2022-10-22-un-article/ | path | slug with ancestors slugs, sans langue |
| /fr/actualites/2022-10-22-un-article/ | permalink |
| /content/posts/2022/2022-10-22-un-article/_index.md | file | Chemin physique du fichier |

## Architecture

Une classe gère globalement les permalinks :
```
class Communication::Website::Permalink
end
```

Responsabilité :
- Configuration Hugo des permalinks
- current (bool et scope)
- historique
- contexte (website)

Un concern Permalinkable qui encapsule le lien (.permalinks, .permalink_in_website)
```
include Permalinkable
```
Responsabilité :
- fournir le permalink actuel
- fournir les anciens
- se déclencher à la synchronisation (afin de gérer la cascade de dépendances en utilisant celle de la sync plutôt que de la répliquer)
- faire le lien avec les objets par type

Des objets par type :
```
class Communication::Website::Permalink::Page
end
```
Responsabilité :
- calcul du permalink
- fonctionnements spécifiques par type d'objet

## Objets directs

### Page

### Post

## Objets indirects

### Program

Le cas de l'arbre des formations et de l'arbre de la page Offre de formation

## Fil d'ariane

En anglais, breadcrumbs.
