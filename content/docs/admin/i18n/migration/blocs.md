---
title: Blocs
weight: 3
---

Il faut adapter les différents blocs de liste pour gérer les localisations. 
On peut distinguer les blocs de liste d'objets directs et indirects.

## Méthode

### 1. Form

Adapter le formulaire d'édition du template de bloc, et par conséquent les partials d'édition des components liés aux objets

### 2. Model

Adapter les modèles de template et de components pour que les scopes de publication et d'ordre prennent en compte la langue (exemple : `published_now_in(block.language)` ou `ordered(block.language)`)

### 3. Snippet

Adapter le snippet pour afficher les contenus localisés

### 4. Show

Adapter le partial `_show` du template pour gérer la prévisualisation ainsi que les extranets

### 5. Static

Adapter le partial `_static` du template pour gérer les localisations des objets listés

### 6. Preview

Le template `preview` de l'objet en question pour utiliser la localisation

## Blocs de liste d'objets directs

- Actualités
- Pages
- Agenda 
- Projets

## Blocs de liste d'objets indirects
- Personnes
- Organisations
- Formations
- Campus
