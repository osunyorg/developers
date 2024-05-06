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

```ruby {filename="app/models/communication/website/connection.rb"}
class Communication::Website::Connection < ApplicationRecord
  # [...]

  protected

  def obsolete?
    !direct_source.recursive_dependencies_include?(indirect_object)
  end
end
```

Côté génération des connexions, il y a une dissonance. Tout d'abord, on boucle sur les objets directs du website, et à chacun d'eux, on appelle la méthode `connect_dependencies`.

```ruby {filename="app/models/communication/website/with_connected_objects.rb"}
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

```ruby {filename="app/models/concerns/as_direct_object.rb"}
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

```ruby {filename="app/models/communication/website/with_connected_objects.rb"}
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

```ruby {filename="app/models/communication/website/with_connected_objects.rb"}
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

### Analyse des différences

Statistiques :
- Après la génération des connexions dans le `clean_and_rebuild` : 12507 connexions
  - dont 1148 avec pour source directe `Communication::Website`
  - dont 5455 avec pour source directe `Communication::Website::Page`
  - dont 5165 avec pour source directe `Communication::Website::Post`
  - dont 674 avec pour source directe `Communication::Website::Post::Category`
  - dont 65 avec pour source directe `Communication::Website::Agenda::Event`
- Après la suppression des connexions obsolètes : 7147 connexions
  - dont 1148 avec pour source directe `Communication::Website`
  - dont 2689 avec pour source directe `Communication::Website::Page`
  - dont 2881 avec pour source directe `Communication::Website::Post`
  - dont 364 avec pour source directe `Communication::Website::Post::Category`
  - dont 65 avec pour source directe `Communication::Website::Agenda::Event`
- Après vérification des objets directs : 6420 connexions
  - dont 1148 avec pour source directe `Communication::Website`
  - dont 1962 avec pour source directe `Communication::Website::Page`
  - dont 2881 avec pour source directe `Communication::Website::Post`
  - dont 364 avec pour source directe `Communication::Website::Post::Category`
  - dont 65 avec pour source directe `Communication::Website::Agenda::Event`
 
Entre les 3 cas, le nombre de connexions est le même pour `Communication::Website` et `Communication::Website::Agenda::Event`.

Enfin, entre les 2 derniers cas, le nombre de connexions est le même pour `Communication::Website::Post` et `Communication::Website::Post::Category`.

Piste possible avec l'analyse précédente, la génération des connexions au niveau d'un post A pouvait suivre les dépendances récursives d'un post B (dépendance directe du post A) et donc le post A héritait des dépendances du le post B.

La seule différence restante entre les derniers cas ont pour source directe `Communication::Website::Page`.

Voici les types d'objets indirects pour les 727 connexions :
- 687 avec `Research::Publication`
- 4 avec `University::Organization`
- 29 avec `University::Person`
- 7 avec `ActiveStorage::Blob`

Sur les 61 pages du site, 55 ont des connexions. Voici les pages ayant un nombre de connexions différentes entre le cas 2 et le cas 3.

- Page « Publications HAL » (`11ca8717-20e7-40bc-b771-fdc661a6977b`) : on passe de 690 à 3 connexions (-687).
  - à savoir les 687 `Research::Publication`
- Page « Organisations » (`936313ef-e28a-4911-b788-40810e8737d3`) : on passe de 10 à aucune connexion (-10).
  - à savoir 6 `ActiveStorage::Blob` et 4 `University::Organization`
- Page « Équipe » (`9c2be9f0-88fc-4e1b-9fd6-2c89fbf1e183`) : on passe de 133 à 103 connexions (-30).
  - à savoir 1 `ActiveStorage::Blob` et 29 `University::Person`

Après avoir constaté cette différence, on découvre que relancer une seconde fois la génération des connexions avec les 6420 connexions initiales remonte ce nombre à 7147, soit le cas 2. On a un problème où, dans les dépendances de ces pages, on récupère les objets par les connexions.

Par exemple, la page Équipe a pour dépendance les connected_people qui sont les objets `University::Person` ayant une connexion sur ce site. On se retrouve à générer des connexions à partir de données venant des connexions.

Idée de piste : Mettre en dépendance uniquement les objets explicitement connectés à la page (pour les personnes et les organisations) et mettre les autres personnes/organisations et les publications en références, pour gérer le cas du changement de slug de la page.

Cependant, si la page spéciale est à l'intérieur d'une autre page, si on change son slug, on change en cascade la page spéciale, et il faudrait également re-synchroniser les objets correspondants.
