---
title: Blocs de contenu
---

Les blocs sont des éléments narratifs qui s'ajoutent à un objet pour le raconter :
sur une page ou une actualité dans un site Web,
sur une formation (dans un site d'école)
sur une école (dans un site d'université)
sur une personne (dans un site).

Les blocs répondent à des besoins plus ou moins sophistiqués, allant d'une image (avec son crédit, son texte alternatif et son alt) à un organigramme faisant référence à des personnes.

Nous utilisons des blocs notamment pour minimiser l'utilisation des éditeurs wysiwyg, qui génèrent du code problématique (HTML non valide).
De plus, ce fonctionnement permet de gérer finement les enjeux d'accessibilité, et les comportements spécifiques (ex: actualités).

Pour faire fonctionner les blocs, il faut :
1. un éditeur
2. un modèle qui liste les dépendances
3. un export statique
4. une prévisualisation (mobile, offcanvas)
5. une gestion des traductions

## 1. Editeur

Les éditeurs sont codés en Vue (parce qu'Angularjs est mort), et doivent permettre une variété d'entrées :
- chaîne de caractère (input text)
- texte long (textarea)
- texte rich (summernote, simple [b, i, ul, ol, a])
- sélecteur natif HTML
- sélecteur custom (liste de layouts)
- nombre
- case à cocher
- upload de fichiers
- éditeur d'objets composites (ex: images dans une galerie)

## 2. Modèle

Le modèle permet de :
- enregistrer les données json dans la base
- sanitizer
- encapsuler les logiques métiers (ex: les partenaires libres vs organisations, les articles non publiés...)
- lister les dépendances

## 3. Export

Les blocs doivent être exportables en Frontmatter pour Hugo.

## 4. Prévisualisation

Une prévisualisation est souhaitable, si possible :
- en temps réel (pas de temps de compilation)
- desktop et mobile
- avec les bons styles

Le précompilé ne permet pas simplement ces 3 souhaits, l'arbitrage actuel est mobile, sans style.

## 5. Traductions

Quelle logique ?
- Master + trads, avec le master qui reste en contrôle (logique "empilée")
- versions autonomes, avec le "master" qui initialise la version, puis chacun vit sa vie (logique "séparée")

## Dev

### Architecture

Le système est développé en 3 parties :
- blocks
- templates
- components

Le block est enregistré en base de donnée, et contient des données json. Il fait le lien avec l'objet auquel il est ajouté, et il définit son template. C'est un objet neutre, tout le spécifique métier est défini dans le template.

Le template répond à un besoin métier (un chapitre, un CTA, une galerie d'images, un organigramme...). Il définit un ensemble de components, qui vont permettre de générer les vues d'édition, de preview, d'export statique et de traduction.

Le component est un objet de bas niveau, comme un champ de texte ou une valeur booléenne. Il correspond à une propriété typée, et propose un éditeur standardisé (comme simple_form), une prévisualisation et un export statique.

### Model

Tous les blocs possèdent un titre, un data en JSONB et une position.

Les attributs dans le JSONB côté Osuny peuvent avoir un type parmi :
* `string` : Champ texte classique
* `text` : Champ textarea
* `richtext (<config>)` : Richtext Summernote avec une configuration définie (`mini`, `mini-list` ou `default`)
  * exemple : `richtext (mini-list)`
* `integer` : Champ de nombre entier
* `float` : Champ de nombre décimal
* `boolean` : Case à cocher (vrai/faux)
* `enum (<value1>, <value2>[, ...])` : Enumération de valeurs possibles
* `references (<model>)` : UUID faisant référence à un objet avec le modèle associé
  * exemple : `references (Communication::Website::Page)`
* `blob` : Champ d'upload de fichier (pour les images notamment), représenté par un objet ayant des attributs `id`, `filename` et `signed_id`
* `array` : Pour un tableau d'éléments
* `hash` : Pour un objet avec des paires clé-valeur


```
communication/Block
- university:references
- about:references (polymorphic)
- template:integer (enum)
- position:integer
- data:jsonb
```

Pour commencer, les valeurs de l'enum seront :
- 100, organization_chart
- 200, partners

### Partial about
Un partial que l'on peut ajouter à un show d'objet, avec :
- la liste des blocs utilisés (avec boutons show et edit)
- la possibilité de les ordonner (position)
- un bouton pour ajouter un bloc

```
views/admin/communication/blocks/_list.html.erb
```

### Show
Le show du bloc utilise le partial de son template
```
views/admin/communication/blocks/templates/partners/_show.html.erb
```

### Edit
L'edit du bloc utilise le partial de son template
```
views/admin/communication/blocks/templates/partners/_edit.html.erb
```

### Concern
Tous les objets ayant des blocs utilisent le concern `WithBlocks`, qui ajoute la méthode `blocks` (la liste des blocs, dans l'ordre).

### Export statique
Les blocs sont exportés dans le frontmatter grâce au partial
```
views/admin/communication/blocks/_static.html.erb
```
qui donne ce type de résultat
```
blocks:
  - template: partners
    data:
      - name: Partner 1
        url: https://partner1.com
        logo: "e09f3794-44e5-4b51-be02-0e384616e791"
```
Les générateurs de chaque type suivent l'organisation :
```
views/admin/communication/blocks/templates/partners/_static.html.erb
```
Attention, il faut 6 espaces pour respecter l'indentation du front-matter :
```
      - name: Partner 1
        url: https://partner1.com
        logo: "e09f3794-44e5-4b51-be02-0e384616e791"
```

### Dépendances

Il faut créer une classe pour chaque template, avec le nom au singulier (partners devient partner.rb, et la class Partner) :

```
models/communication/block/partner.rb
```
avec une structure de type :
```
class Communication::Block::Partner < Communication::Block::Template
  def git_dependencies
    ...
  end
end
```

### Blocs dans l'extranet

*En construction*

Les blocs sont utilisés dans 4 contextes :
- Site Web en prod
- Extranet en prod
- Preview admin dans un contexte Site Web
- Preview admin dans un contexte Extranet

Dans les previews dans les sites, on utilise le style du site en allant les récupérer via l'URL du website.

Dans les previews dans les extranets, il faut utiliser le style de l'extranet.

Actuellement :
- Les extranets et les blocs de preview sont intégrés avec Bootstrap. Or, dans le thème, les blocs sont intégrés sans Bootstrap.
- Les DOMs des blocs sont à maintenir dans la partie back Osuny et dans la partie thème Osuny (et ça ne bouge pas).

Plusieurs solutions :
- Sortir la partie Bootstrap des extranets (il faut une *grande* ré-intégration front)
- Exposer un ficher de style spécial pour les blocs sur le site Example pour l'importer dans les extranets

## Pour créer un bloc

1. Déclarer le template dans l'enum du modèle block
2. Créer l'edit, le show et le static dans la vue du template
3. Créer la classe du template pour gérer les dépendances
4. Créer la vignette dans les images
5. Ecrire les locales fr et en
