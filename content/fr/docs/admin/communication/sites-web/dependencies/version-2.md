---
title: Version 2
description: Implémentée en 2023
---

Le point de départ de cette version est multiple :
- résoudre les boucles infinies
- permettre l'indirect non listé
- éviter les commits multiples lors d'une seule action
- simplifier la maintenance

Plusieurs intuitions guident cette version :
1. lister les dépendances avec un algorithme récursif qui évite les boucles infinies
2. inscrire les connexions en base de données
3. traiter clairement les liens entre objets et sites Web
4. consolider chaque nuit les connexions (ce qui traite aussi la publication d'actualité dans le futur)

Cette version se concentre sur l'intégrité et la robustesse, en laissant de côté (pour une v3 ?) la précision des événements déclencheurs.

## 1. La liste de dépendances

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
  # Ces 2 méthode doivent être définies dans chaque objet
  
  # Dépendances d'affichage
  def display_dependencies
    []
  end

  # Dépendances de référence
  def reference_dependencies
    []
  end

  def dependencies(array = [])
    display_dependencies.each do |dependency|
      next if dependency.in?(array)
      array << dependency
      next unless dependency.respond_to?(:dependencies)
      array += dependency.dependencies(array)
    end
    reference_dependencies.each do |dependency|
      next if dependency.in?(array)
      array << dependency
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
  def display_dependencies
    # Les images à la une, héritées ou pas
    active_storage_blobs + 
    # Les blocks (pas besoin de lister les dépendances des blocs, c'est récursif)
    blocks 
  end

  def reference_dependencies
    # Les items de menu liés à cette page
    menu_items +
    # Les enfants en cas de changement de path
    descendants +
    # Le parent (pour lister les enfants)
    [parent]
  end
```

La définition des dépendances d'affichage (`display_dependencies`) est particulièrement délicate. 
Il faut être strict, sinon on arrive vite à mettre tout en dépendance de tout.
La question de la nécessité pour l'affichage doit guider les choix.

Quelques exemples...
- Le `parent` n'est pas une dépendance d'affichage, parce qu'il n'est pas nécessaire pour afficher la page.
- Les enfants `children` ne sont pas des dépendances, parce qu'ils ne sont pas nécessaires pour afficher la page.
En revanche, si un bloc "Pages" mentionne des pages, elles sont des dépendances parce qu'il faut les envoyer pour afficher la page complètement.
- Si on a le parent et les enfants, en fait toutes les pages sont reliées entre elles.

## 2. Les connexions

### Principe
```
Communication::Website::Connection
#  id                  :uuid             not null, primary key
#  university_id       :uuid             not null, indexed
#  website_id          :uuid             not null, indexed
#  object_type         :string           indexed => [object_id]
#  object_id           :uuid             indexed => [object_type]
#  created_at          :datetime         not null
#  updated_at          :datetime         not null
```

On inscrit la connexion dans l'université et dans le site Web.
On mentionne l'objet polymorphe (page, personne, blob...).
Les connexions permettent de retrouver dans quel site est utilisé un objet.

### Implémentation

```ruby
Communication::Website
  WithDependencies
  WithConnections
  
  def display_dependencies
    pages +
    posts + 
    categories +
    menus +
    [about]
  end
```

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

  def connect(object)
    connect_object object
    object.dependencies.each do |dependency|
      connect_object dependency
    end
  end

  # TODO pas pensé
  def disconnect(object)
    disconnect_object object
    object.dependencies.each do |dependency|
      disconnect_object dependency
    end
  end

  protected

  def connect_object(object)
    connection = connections.where(university: university, object: object).first_or_create
    connection.touch if connection.persisted?
  end

  def disconnect_object(object)
    connections.where(university: university, object: object).destroy_all
  end
```

## 3. Le lien objet / sites Web

### Cas

1. Quand on crée une personne, elle n'est liée à aucun site Web
2. Quand on ajoute une personne à un site explicitement (en passant par la page Équipe), ça crée une connexion
3. Quand on ajoute une personne à un bloc "Personnes" d'une page, ça crée une connexion avec le site de la page
4. Quand on ajoute une personne à un bloc "Personnes" d'une formation, ça crée une connexion avec tous les sites

Une personne se lie à un site parce qu'on l'ajoute à un objet qui est lié à un site.
Quand on enregistre un bloc, il faut vérifier si l'objet auquel appartient le bloc est connecté à des websites.

### Implémentation

Le bloc déclenche la connection de son objet de rattachement (`about`), qui le listera en retour dans ses dépendances. 
```ruby
module WithWebsites
  extend ActiveSupport::Concern

  included do 
    after_save :connect_to_websites
  end

  def websites
    @websites ||= Communication::Website::Connection.websites_for self
  end

  protected
  
  def connect_to_websites
    if respond_to?(:website) && !website.nil?
      website.connect self
    else
      websites.each { |website| website.connect self }
    end
  end
end
```

## 4. Le nettoyage nocturne
Quotidiennement, après minuit, on reconstruit les connexions du site Web pour vérifier l'intégrité et réparer d'éventuels problèmes.
Cela permet de traiter la publication des actualités dans le futur, puisque les publications prévues pour le jour concerné seront ajoutées avec leurs connexions.

À la fin du traitement, il faut supprimer les connexions qui n'ont pas été touchées pendant le processus, puisque cela signifie qu'elles sont obsolètes.
C'est un genre de "garbage collector" des connexions, qui évite d'envoyer de vieux objets un jour après leur suppression.

La suppression des connexions obsolètes coupera le lien avec les objets qui ne sont plus liés au site.
