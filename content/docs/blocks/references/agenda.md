---
title: Agenda
weight: 5
---

![image](https://raw.githubusercontent.com/osunyorg/admin/refs/heads/main/app/assets/images/communication/blocks/templates/agenda.jpg)

```yaml {filename="Données Hugo"}
  - kind: block
    template: agenda
    title: >-
      À venir
    slug: >-
      a-venir
    ranks:
      self: 2
      children: 3
    data:
      mode: all
      options:
        categories: false
        dates: true
        image: true
        subtitle: true
        summary: true
        status: false

      layout: grid
      description: >-
        
      no_event_message: >-
        
      title_link: "/agenda/"
      events:
        - permalink: "/agenda/2024-11-27-communs-numerique-et-interet-general/"
          path: "/events/2024-12-27-communs-numerique-et-interet-general"
          slug: "communs-numerique-et-interet-general"
          file: "content/fr/events/2024-12-27-communs-numerique-et-interet-general.html"
```

## Gestion des créneaux horaires

Les créneaux impliquent un traitement particulier, pour éviter les déceptions.
Un créneau appartient toujours à un événement, et s'inscrit entre la date de début et la date de fin.
Dans le bloc agenda, on peut afficher les événements du jour, ou les événements futurs.
Il ne faut pas donner de fausses informations, évidemment.

### Aujourd'hui

#### Créneau unique
Il faut afficher le créneau s'il tombe aujourd'hui, et masquer l'événement sinon.

#### Créneaux multiples sur plusieurs jours
Il faut afficher uniquement le créneau du jour.
S'il n'y a aucun créneau aujourd'hui, on masque l'événement.

#### Créneaux multiples sur le même jour
Il faut afficher une entrée pour chaque créneau du jour, avec l'heure.

> Exemple :
> Séance à 14h
> Séance à 16h

### Aujourd'hui et dans le futur
Il faut afficher les créneaux à la place des événements, dans le flux temporel.

