---
title: Version 4
description: Réflexion en cours
---

Le point de départ de cette version reste identique à la version 2:
- résoudre les boucles infinies
- permettre l'indirect non listé
- éviter les commits multiples lors d'une seule action
- simplifier la maintenance

Toute cette démarche est exclusivement liée aux sites Web dans le cadre de l'export des fichiers statiques vers Git.

Il y a 2 natures d'objets :
- les objets directs, dotés d'un website_id (pages, posts, catégories)
- les objets indirects (organisations, personnes, blocs...)

Les dépendances sont les objets nécessaires pour l'affichage de l'objet courant :
- images
- blocs
- personnes listées dans les blocs
- ...
Cette liste est construite récursivement.

Les références sont les objets qui citent (ou peuvent citer) l'objet courant :
- éléments de menu
- parents
- enfants
- blocs de listes
- ...

Les connexions créent des liens entre les objets indirects et les sites Web, par le biais d'un objet direct :
- un bloc par une page
- une personne par le bloc de la page
- une personne par lien direct au site (via la page équipe)
- une école par lien direct au site (about pour un site d'école)
- une formation par son école
- une formation par lien direct au site (about pour un site de formation)
- ...
L'objet direct s'appelle "source“.
Un objet peut être connecté par plusieurs sources.

Attention au cas de la dénormalisation des websites dans les blocs : il faut choisir si on considère le bloc comme un objet direct ou pas.

## Les dépendances

### Principe

Pour éviter la boucle infinie, il faut écrire un algorithme capable de suivre la chaîne de dépendance sans tomber dans la boucle infinie :
- pour chaque dépendance d'affichage, vérifier si elle est déjà traitée
  - si non, l'ajouter et reprendre avec les enfants
  - si oui, l'ignorer et ignorer les enfants
- pour chaque dépendance de référence, vérifier si elle est déjà traitée
  - si non, l'ajouter
  - si oui, l'ignorer

Pour respecter les principes de [responsabilité unique](https://en.wikipedia.org/wiki/Single-responsibility_principle) et d'[encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)), cet algorithme :
- ne doit pas se préoccuper de site Web (pas de contexte)
- ne doit pas fouiller dans ses propres dépendances (chacun s'occupe de son niveau)

### Implémentation

```ruby
concerns/WithDependencies
  # Déclaration à faire dans les objets
  def dependencies
    []
  end

  def recursive_dependencies(array = [])
    dependencies.each do |dependency|
      next if dependency.in?(array)
      array << dependency
      next unless dependency.respond_to?(:recursive_dependencies)
      array += dependency.recursive_dependencies(array)
    end
    array
  end
```

Warning : vérifier que `dependency.in?(list)` ne donne pas de faux négatifs à cause d'active record.
Par exemple, j'instancie 2 fois le même auteur, mais les objets sont distincts en RAM, donc interprétés comme différents.
A mettre sous test.

### Exemple pour une page
```ruby
Communication::Website::Page
  include WithDependencies

  def dependencies
    # Les images à la une, héritées ou pas
    active_storage_blobs + 
    # Les blocks (pas besoin de lister les dépendances des blocs, c'est récursif)
    blocks 
  end
```

La définition des dépendances est particulièrement délicate. 
Il faut être strict, sinon on arrive vite à mettre tout en dépendance de tout.
La question de la nécessité pour l'affichage doit guider les choix.

Quelques exemples...
- Le `parent` n'est pas une dépendance d'affichage, parce qu'il n'est pas nécessaire pour afficher la page.
- Les enfants `children` ne sont pas des dépendances, parce qu'ils ne sont pas nécessaires pour afficher la page.
En revanche, si un bloc "Pages" mentionne des pages, elles sont des dépendances parce qu'il faut les envoyer pour afficher la page complètement.
- Si on a le parent et les enfants, en fait toutes les pages sont reliées entre elles.

## Les références

### Principe

Lister tous les objets qui citent l'objet courant, pour pouvoir les mettre à jour si l'objet change de path par exemple.
À l'idéal, seules certaines opérations déclenchent la mise à jour des références.
Dans un premier temps, on se préoccupe uniquement de lister tout correctement, pas d'optimiser.

### Implémentation

```ruby
concerns/WithReferences

  def references
    []
  end
```

### Exemple pour une page

```ruby
Communication::Website::Page
  include WithReferences

  def references
    # Les items de menu liés à cette page
    menu_items +
    # Les enfants en cas de changement de path
    descendants +
    # Le parent (pour lister les enfants)
    [parent]
  end
```

## Les connexions

### Principe

Tous les objets indirects génèrent une connexion quand ils sont ajoutés aux dépendances d'un objet direct.
Cela permet de répondre aux questions, au sujet d'un objet indirect (personne, organisation...) : 
- dans quels sites l'objet est-il utilisé ?
- dans quel objet direct de ce site l'objet est-il utilisé (quelle page ? quelle actu ?)
- quels objets indirects sont connectés à un site ?

Quand on sauve un objet indirect, il faut faire 3 actions :
- lister les sources et s'y connecter
- connecter toutes les dépendances à toutes les sources
- réenregistrer toutes les sources (donc de tous les websites)

Pour lister les sources, on passe par les connexions, ce qui pose un problème d'œuf et de poule.
Pour créer l'œuf, il faut trouver dans les références toutes les connexions, puis créer des connexions avec la même source directe.
C'est un système viral : un object connecté connecte ses dépendances et ses références.

Les connexions sont établies peu importe si l'object indirect doit être envoyé sur Git ou non. 
Par exemple, un Communication::Block d'une page peut être connecté à un site par cette page mais il sera envoyé sur Git selon l'état de publication du bloc (`block.published?`).

### Implementation

```
Communication::Website::Connection
#  id                  :uuid             not null, primary key
#  university_id       :uuid             not null, indexed
#  website_id          :uuid             not null, indexed
#  object_type         :string           indexed => [object_id]
#  object_id           :uuid             indexed => [object_type]
#  source_type         :string           indexed => [source_id]
#  source_id           :uuid             indexed => [source_type]
#  created_at          :datetime         not null
#  updated_at          :datetime         not null
```

On inscrit la connexion dans l'université et dans le site Web.
On mentionne l'objet polymorphe (page, personne, blob...).
Les connexions permettent de retrouver dans quel site est utilisé un objet.

```ruby
module Communication::Website::WithConnections
  extend ActiveSupport::Concern

  included do
    has_many  :connections

    after_save :clean_connections!
  end

  def clean_connections!
    start = Time.now
    connect self
    connections.where('updated_at < ?', start).destroy_all
  end

  def connect(object, source)
    connect_object object, source
    object.recursive_dependencies.each do |dependency|
      connect_object dependency, source
    end
  end

  def disconnect(object, source)
    disconnect_object object, source
  end

  protected

  def connect_object(object, source)
    connection = connections.where(university: university, object: object, source: source).first_or_create
    connection.touch if connection.persisted?
  end

  def disconnect_object(object, source)
    connections.where(university: university, object: object, source: source).destroy_all
  end
```

```ruby
Communication::Website
  WithDependencies
  WithConnections
  
  def dependencies
    pages +
    posts + 
    categories +
    menus +
    [about]
  end
```

## Suppressions

Quand on supprime un objet direct, il faut supprimer toutes les connexions dont il est la source.
En revanche, quand on déconnecte un objet indirect, il faut reconstruire les connexions des sources.

En fonction de la rapidité d'exécution, cela se fera soit : 
- en synchrone (peu probable)
- en asynchrone immédiat
- en asynchrone nocturne


