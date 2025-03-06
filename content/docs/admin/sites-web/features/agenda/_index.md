---
title: Agenda
weight: 2
---
*Gérer un calendrier d'événements*

Est-ce vraiment du domaine du site ? Ou est-ce plus haut, avec une publication sur le site ? Par souci de simplicité, on développe les événements au sein des sites Web uniquement. S'ils sont développés aussi au sein des extranets, il est probable que les enjeux fonctionnels soient différents (inscription notamment). À la v1 de l'agenda, produite pour La Criée et l'appel d'offres Villa Médicis, s'ajoute la v2 produite pour la Gaîté Lyrique.

## Événements

### Simple

Un concert unique, une conférence...
Pas de créneaux du tout (journée) ou 1 créneau horaire unique.
Pas d’enfants.

Exemple
- Alaska : sur la piste de Telaquana (20 janvier 2025)

| Hugo | Valeur | Exemple |
| - | - | - |
| file | /content/fr/events/YYYY/MM/DD-slug.html | /content/fr/events/2025/01/20-alaska.html |
| permalink | /fr/agenda/YYYY/MM/DD-slug/ | /fr/agenda/2025/01/20-alaska/ |
| path | ? | ? |

### Récurrent

Une série de séances sur une journée ou une série de concerts sur plusieurs jours.
Créneaux horaires multiples.
Pas d’enfants.

Les événements récurrents s'appuient sur des time slots (`Agenda::Event::TimeSlot`), qui génèrent des fichiers statiques.

Exemples
- Podcast encore heureux (19h et 20h30 le même jour)
- Contes à paillettes

Pour chaque time slot, il faut générer un fichier pour affichage dans la liste.
Comment gérer le premier ?
L'événement est-il envoyé ?

| Hugo | Valeur | Exemple |
| - | - | - |
| file | /content/fr/events/YYYY/MM/DD-HH-mm-slug.html |
| permalink | /fr/agenda/YYYY/MM/DD-slug/ |
| path | ? | ? |

### Parent

Pas de créneaux.
Enfants (n>1, le cas 1 enfant devrait être traité en événement simple).
Une journée d’études avec des conférences successives ou un festival de musique.


Afin d'afficher les enfants nichés sous les parents dans les listes, il faut utiliser un objet jour (`Agenda::Event::Day`), qui modélise un jour d'un festival.
Ainsi, l'Arte Concert Festival aura 3 jours, et chaque jour génère un fichier statique afin de permettre à Hugo de retrouver ses petits.


Exemples
- Rencontres Internationales Paris-Berlin
- Arte Concert Festival
- Ciné Club Gaze


| Hugo | Valeur |
| - | - |
| file | /content/fr/events/YYYY/slug.html |
| permalink | /fr/agenda/YYYY/slug/ |

### Enfant

Concert pendant un festival.
Un parent, pas d'enfant (pas d’enfant d’enfant).
En général avec un créneau, les cas 0 (pas d’horaire) et n>1 (récurrent) sont possibles.


Exemple
- Concert de Priya Ragu pendant Arte Concert Festival

## Expositions

Une exposition, ouverte tous les jours.
Jour de fin > jour de début.
Pas de créneaux, pas d’enfants.
Rien = tout (ouvert tous les jours).

Exemple :
- PULSE : Au rythme de la lumière et du son (Du 12 décembre 2024 au 13 juillet 2025)

| Hugo | Valeur | Exemple |
| - | - | - |
| file | /content/fr/exhibitions/YYYY/slug.html | /content/fr/exhibitions/2024/pulse.html |
| permalink | /fr/expositions/YYYY/slug/ | /fr/expositions/2024/pulse/ |
| path | ? | ? |

## Catégories

Une caractérisation des événements, par exemple un congrès ou un concert. Un événement peut avoir un et un seul type. Le fait d'avoir un élément typé permet de filtrer sur le site Web.

## Périodes

Pour avoir des sortes de filtres de calendrier, il est nécessaire de générer des pages pour les années et les mois.
La page d'un mois liste les jours, avec les événements de chaque jour.
La page d'une année liste les mois.

## Lieu (pas fait dans la v2)

Un lieu, physique ou virtuel, par exemple l'IUT Bordeaux Montaigne, une chaîne Twitch ou une url de webinaire. Le lieu peuvent avoir des enfants, par exemple des salles à l'IUT, qui sont au sein d'un bâtiment. Un événement peut être connecté à plusieurs lieux, comme c'est le cas pour les événements hybrides.

## Exemples

- https://www.college-de-france.fr/fr/agenda
- https://www.college-de-france.fr/fr/agenda/colloque/origines-des-planetes-la-vie
- https://www.college-de-france.fr/fr/agenda/colloque/origines-des-planetes-la-vie/interventions-institutionnelles-mesr-cnrs
- https://www.louvre.fr/expositions-et-evenements/expositions
- https://www.louvre.fr/expositions-et-evenements/evenements-activites
