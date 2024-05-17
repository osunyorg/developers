---
title: Itération 10
description: Réflexion 17 mai 2024
weight: 10
---

Le processus global de synchronisation Git est lent, et il y a des trous à la suppression.

2 gros sujets de lenteur :
- la régénération des statics à chaque analyse, pour avoir leur SHA
- la vérification d'obsolescence des connexions

## Cacher les statics

Une vérif de mise en cache a été tentée par PA et Seb (sans gérer les règles fines), et on obtient un gain de facteur 4.

### Clés

```
[website_id]-[object-identifier]-static
[website_id]-[object-identifier]-sha
```

### Fonctionnement

Il faudrait mettre les contenus et les SHA dans Redis.

Création des clés à l'appel (lazy).

Le pb est de buster le cache des bonnes clés.
Si on bust trop, on recalcule pour rien (pb d'optimisation).
Si on bust pas assez, on ne met pas à jour (pb d'intégrité).
Il vaut mieux buster trop que pas assez.

L'idée est de buster les clés de toutes les dépendances d'un objet qu'on enregistre.

## Améliorer la vérification des connexions

Chaque objet `Connection` gère sa propre obsolescence, ce qui cause des dizaines de milliers de tâches chaque jour.
L'objet monte en RAM ses dépendances, pour conclure dans la plupart des cas que tout va bien.

### Regrouper au niveau du site

```ruby {filename="app/models/communication/website/connection.rb"}
  def destroy_if_obsolete
    destroy if obsolete?
  end
  handle_asynchronously :destroy_if_obsolete, queue: :cleanup
```

devient 

```ruby {filename="app/models/communication/website/connection.rb"}
  def destroy_if_obsolete
    destroy if obsolete?
  end
```

`Communication::CleanWebsitesJob` doit en revanche faire 1 tâche par site, pour cloisonner les risques et limiter l'usage de RAM.

### Regrouper par heure

L'idée est de passer à une tâche toutes les heures (il n'y a pas le feu au lac).
Au lieu de laisser chaque objet se vérifier lui-même, on marque les objets possiblement impactés.
Lors de la vérification, s'il y a des objets marqués, on évalue leur obsolescence dans 1 tâche par site.

### Regrouper par `direct_source`

Les connexions sont analysées pour un objet indirect et une source directe.
À chaque fois, on recharge les dépendances de l'objet direct.
Il vaut donc mieux analyser toutes les connexions d'un objet direct d'un coup, donc créer des tâches par objet direct.


Pour éviter de traiter les objets directs qui n'ont pas de connexion, il faut passer par les sources des connexions.


Le website étant une `direct_source`, nous n'avons plus besoin d'analyser les connexions au niveau du website.

```ruby {filename="app/models/communication/website/connection.rb"}
  def self.delete_useless_connections(direct_object, dependencies)
    deletable_connection_ids = []
    direct_object.connections.find_each do |connection|
      deletable_connection_ids << connection.id if connection.obsolete_in?(dependencies)
    end
    # On utilise delete_all pour supprimer les connexions obsolètes en une unique requête DELETE FROM
    # Cependant, on peut le faire car les connexions n'ont pas de callback.
    # Dans le cas où on en rajoute au destroy, il faut repasser sur un appel de destroy sur chaque
    direct_object.connections.where(id: deletable_connection_ids).delete_all
  end

  protected

  # On passe les dépendances pour ne pas les recharger et préserver la RAM 
  def obsolete_in?(dependencies)
    dependencies.detect { |dependency|
      dependency.class.name == indirect_object_type &&
      dependency.id == indirect_object_id
    }
  end
```

```ruby {filename="app/models/concerns/as_direct_object.rb"}
  # L'objet fait son ménage
  # Cette méthode est appelée dès qu'on enregistre un objet indirect, sur chaque `direct_source` connectée (via les connexions)
  def delete_obsolete_connections
    Communication::Website::Connection.delete_useless_connections(self, recursive_dependencies)
  end
```

```ruby {filename="app/models/communication/website/with_connected_objects.rb"}
  # Le site fait le ménage de ses connexions directes uniquement
  def delete_obsolete_connections
    # On prend l'about et ses dépendances récursives
    # On ne prend pas toutes les dépendances parce qu'on s'intéresse uniquement à la connexion via about
    about_dependencies = [about] + about.recursive_dependencies
    Communication::Website::Connection.delete_useless_connections(self, about_dependencies)
  end

  # Le site fait son ménage de printemps
  # Appelé
  # - par un objet avec des connexions lorsqu'il est destroyed
  # - par le website lui-même au changement du about
  def delete_obsolete_connections_for_self_and_direct_sources
    direct_source_ids_per_type_through_connections.each do |direct_source_type, direct_source_ids|
      direct_sources = direct_source_type.safe_constantize.where(id: direct_source_ids)
      direct_sources.find_each(&:delete_obsolete_connections)
    end
  end

  protected

  # On passe par les connexions pour éviter d'analyser des objets directs qui n'ont pas d'objets indirects du tout
  # Le website lui même est inclus dans le retour (s'il a un about qui déclenche des connexions)
  def direct_source_ids_per_type_through_connections
    # { 
    #  'Communication::Website::Post': ['ID1', 'ID2', ...], 
    #  'Communication::Website::Page': ['ID1', 'ID2', ...], 
    #  'Communication::Website': ['ID1'],
    #  ...
    # }
    connections.group(:direct_source_type)
               .pluck(:direct_source_type, Arel.sql('array_agg(DISTINCT direct_source_id)'))
               .to_h
  end
```

