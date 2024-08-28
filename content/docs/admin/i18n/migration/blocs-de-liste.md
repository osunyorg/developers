---
title: Blocs de liste
weight: 4
---

Il faut également adapter les différents blocs de liste pour gérer les localisations. On peut distinguer deux catégories :
- les blocs de liste d'objets directs : les blocs Actualités, Pages, Agenda et Projets
- les blocs de liste d'objets indirects : les blocs Personnes, Organisations, Formations et Campus

Il faut :
- adapter le formulaire d'édition du template de bloc, et par conséquent les partials d'édition des components liés aux objets
- adapter les modèles de template et de components pour que les scopes de publication et d'ordre prennent en compte la langue (exemple : `published_now_in(block.language)` ou `ordered(block.language)`)
- adapter le snippet pour afficher les contenus localisés
- adapter le partial `_show` du template pour gérer la prévisualisation ainsi que les extranets
- adapter le partial `_static` du template pour gérer les localisations des objets listés
- le template `preview` de l'objet en question pour utiliser la localisation
