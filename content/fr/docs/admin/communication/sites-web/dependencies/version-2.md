---
title: Version 2
description: Implémentée en 2023
---

Le point de départ de cette version est multiple :
- résoudre les boucles infinies
- permettre l'indirect non listé
- simplifier la maintenance


## Les connexions
L'intuition est de ne pas lister à la volée, mais d'inscrire les dépendances en base de données.

```
Communication::Website::Connections
#  id                  :uuid             not null, primary key
#  university_id       :uuid             not null, indexed
#  website_id          :uuid             not null, indexed
#  object_type         :string           indexed => [object_id]
#  object_id           :uuid             indexed => [object_type]
#  block_id            :uuid             indexed
#  source_type         :string           indexed => [source_id]
#  source_id           :uuid             indexed => [source_type]
#  created_at          :datetime         not null
#  updated_at          :datetime         not null
```
### Le contexte : université et site Web
On inscrit la connexion dans l'université et dans le site Web.

### L'objet
On mentionne l'objet polymorphe (page, personne, blob...).

### Le bloc
Le bloc est un lien optionnel, si l'objet est lié dans le cadre d'un bloc.

### La source
La source est l'entité dont l'objet est la dépendance :
- l'image d'une page a pour source la page
- l'image d'une personne a pour source la personne
- la personne a pour source l'objet auquel est rattaché le bloc

## L'algorithme
Pour cela, il faut écrire un algorithme capable de suivre la chaîne de dépendance sans tomber dans la boucle infinie :
- prendre la liste de dépendance directe
- pour chaque dépendance, vérifier si elle est déjà traitée
  - si oui, l'ignorer et ignorer les enfants
  - si non, l'ajouter et reprendre avec les enfants

