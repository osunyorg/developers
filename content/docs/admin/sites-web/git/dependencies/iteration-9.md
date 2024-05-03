---
title: Itération 9
description: Réflexion 3 mai 2024
---

## Durée de vérification d'obsolescence d'une connexion

La tâche nocturne s'occupant de clean et rebuild les sites web regénèrent les connexions à partir des objets directs du site et du about du site. Ensuite, on appelle une méthode qui va vérifier si chaque connexion d'un site web est obsolète.

Chaque vérification de connexion se fait dans une tâche de fond séparément, et charge toutes les dépendances récursives de l'objet direct pour vérifier que l'objet indirect est présent.

Pour optimiser cette partie, on crée une méthode qui s'occupe de s'arrêter dès que l'objet est trouvé pendant le chargement des dépendances récursives pour éviter de charger l'ensemble dans le cas où l'objet est le 3e trouvé par exemple.

## Quantité de connexions

Lors du travail du 1er point, on teste avec les connexions du site de l'IUT Bordeaux Montaigne (au 3 mai 2024).

Voici les statistiques :
- Après la génération des connexions dans le `clean_and_rebuild` : 12507 connexions
- Après la suppression des connexions obsolètes : 7147 connexions

On peut en déduire une incohérence dans les algorithmes de chaque côté.

Après analyse, la suppression des connexions se base sur la présence de l'objet indirect dans les dépendances récursives de l'objet direct.

```ruby
class Communication::Website::Connection < ApplicationRecord
  # [...]

  protected

  def obsolete?
    !direct_source.recursive_dependencies_include?(indirect_object)
  end
end
```

Côté génération des connexions, il y a une dissonance. Tout d'abord, on boucle sur les objets directs du website, et à chacun d'eux, on appelle la méthode `connect_dependencies`.

```ruby
module Communication::Website::WithConnectedObjects
  extend ActiveSupport::Concern

  # ...

  protected

  # ...

  def clean_and_rebuild_safely
    direct_objects_association_names.each do |association_name|
      # We use find_each to avoid loading all the objects in memory
      public_send(association_name).find_each(&:connect_dependencies)
    end
    # ...
  end
end
```

Cette méthode va récupérer les dépendances de 1er niveau de chaque objet direct et la connecter au website avec pour source l'objet direct en question.

```ruby
module AsDirectObject
  extend ActiveSupport::Concern

  # ...

  def connect_dependencies
    dependencies.each do |dependency|
      website.connect(dependency, self)
    end
  end
end
```

La méthode `connect` va tout d'abord connecter la dépendance si c'est connectable, puis si elle a des dépendances récursives, on va les connecter également. Cependant, on va parcourir ces mêmes dépendances dans la cas où on traite un objet direct alors qu'on ne les suit pas dans `Communication::Website::Connection#obsolete?`.

```ruby
module Communication::Website::WithConnectedObjects
  extend ActiveSupport::Concern

  # ...

  def connect(indirect_object, direct_source, direct_source_type: nil)
    connect_object indirect_object, direct_source, direct_source_type: direct_source_type
    return unless indirect_object.respond_to?(:recursive_dependencies)
    indirect_object.recursive_dependencies.each do |dependency|
      connect_object dependency, direct_source
    end
  end
end
```

On rajoute alors un early return dans le cas où l'objet est un objet direct.

```ruby
module Communication::Website::WithConnectedObjects
  extend ActiveSupport::Concern

  # ...

  def connect(indirect_object, direct_source, direct_source_type: nil)
    # Ajout d'un early return
    return if indirect_object.try(:is_direct_object?)
    connect_object indirect_object, direct_source, direct_source_type: direct_source_type
    return unless indirect_object.respond_to?(:recursive_dependencies)
    indirect_object.recursive_dependencies.each do |dependency|
      connect_object dependency, direct_source
    end
  end
end
```

Avec ça, en vidant les connexions de l'IUT Bordeaux Montaigne et en les régénérant, on obtient 6420 connexions (contre 7147 avant). À noter qu'après passage de la suppression des connexions obsolètes, on reste à 6420 connexions.

Il faut désormais analyser si les 727 connexions perdues sont pertinentes ou non.