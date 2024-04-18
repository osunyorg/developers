---
title: Agenda
weight: 2
---
*Gérer un calendrier d'événements*

Est-ce vraiment du domaine du site ? Ou est-ce plus haut, avec une publication sur le site ? Par souci de simplicité, on développe les événements au sein des sites Web uniquement. S'ils sont développés aussi au sein des extranets, il est probable que les enjeux fonctionnels soient différents (inscription notamment).

## Structure

### Evénément (Agenda::Event)

L'objet de base, qui peut recouvrir de nombreuses réalités aux vocabulaires non stabilisés (festival, congrès, colloque, concert, table ronde, débat, conférence, session...). L'événement peut avoir des éléments enfants, comme par exemple un festival et des concerts, ou un colloque et des conférences.

### Type (Agenda::Kind)

Une caractérisation des événements, par exemple un congrès ou un concert. Un événement peut avoir un et un seul type. Le fait d'avoir un élément typé permet de filtrer sur le site Web.

### Thème (Agenda::Theme)

Une conférence peut parler de "Dispositifs médiatiques" ou de "Culture(s), création et innovation", quand un concert peut être un concert de "Rap" ou de "Rock". En toute rigueur, les thèmes sont en arbre aussi (parents-enfants), même s'il est probable que les sous-niveaux soient rarement nécessaires.

## Catégorie (Agenda::Category)

Remplace Kind et Theme

### Lieu (Agenda::Place)

Un lieu, physique ou virtuel, par exemple l'IUT Bordeaux Montaigne, une chaîne Twitch ou une url de webinaire. Le lieu peuvent avoir des enfants, par exemple des salles à l'IUT, qui sont au sein d'un bâtiment. Un événement peut être connecté à plusieurs lieux, comme c'est le cas pour les événements hybrides. 

## Exemples

- https://www.college-de-france.fr/fr/agenda
- https://www.college-de-france.fr/fr/agenda/colloque/origines-des-planetes-la-vie
- https://www.college-de-france.fr/fr/agenda/colloque/origines-des-planetes-la-vie/interventions-institutionnelles-mesr-cnrs

## Modèles

### communication/website/agenda/Event

- university:references
- communication_website_events_event:references
- communication_website_event_kind:references
- name:string
- description:text
- text:html
- published:boolean
- published_at:datetime

### communication/website/agenda/Category

- university:references
- communication_website:references
- name:string
- position:integer
