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

L'idée est de passer à une tâche toutes les heures (il n'y a pas le feu au lac).
Au lieu de laisser chaque objet se vérifier lui-même, on marque les objets possiblement impactés.
Lors de la vérification, s'il y a des objets marqués, on évalue leur obsolescence dans 1 tâche par site.

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

