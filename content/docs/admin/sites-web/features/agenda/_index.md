---
title: Agenda
weight: 2
---
*Gérer un calendrier d'événements*

La fonctionnalité permet de gérer un calendrier d'événements et d'expositions, avec tous les cas courants.

## Événements

{{< filetree/container >}}
  {{< filetree/folder name="content" >}}
    {{< filetree/folder name="events" state="open" >}}
      {{< filetree/folder name="archives" state="closed" >}}{{< /filetree/folder >}}
      {{< filetree/folder name="2025" state="open" >}}
        {{< filetree/folder name="01" state="open" >}}
          {{< filetree/file name="08-alaska.html" >}}
          {{< filetree/file name="14-16-00-contes-a-paillettes.html" >}}
          {{< filetree/file name="20-16-00-contes-a-paillettes.html" >}}
          {{< filetree/file name="20-19-00-encore-heureux.html" >}}
          {{< filetree/file name="20-20-30-encore-heureux.html" >}}
          {{< filetree/file name="24-20-30-anetha-vel.html" >}}
          {{< filetree/file name="_index.html" >}}
        {{< /filetree/folder >}}
        {{< filetree/folder name="arte-concert-festival" state="open" >}}
          {{< filetree/file name="2025-01-01-20-00-priya-ragu.html" >}}
          {{< filetree/file name="2025-01-02-20-00-gonzales.html" >}}
          {{< filetree/file name="2025-01-03-20-00-gonzales.html" >}}
          {{< filetree/file name="_index.html" >}}
        {{< /filetree/folder >}}
        {{< filetree/folder name="cine-club-gaze" state="open" >}}
          {{< filetree/file name="2025-01-05-20-00-emmanuelle.html" >}}
          {{< filetree/file name="2025-01-15-20-00-atlantique.html" >}}
          {{< filetree/file name="_index.html" >}}
        {{< /filetree/folder >}}
        {{< filetree/folder name="paris-berlin" state="open" >}}
          {{< filetree/file name="2025-01-10-14-00-les-fantomes-de-la-liberte.html" >}}
          {{< filetree/file name="2025-01-10-16-00-resistance-fondamentale.html" >}}
          {{< filetree/file name="_index.html" >}}
        {{< /filetree/folder >}}
        {{< filetree/file name="_index.html" >}}
      {{< /filetree/folder >}}
      {{< filetree/file name="_index.html" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

### Simple

Un concert unique, une conférence...<br>
Pas de créneaux du tout (journée) ou 1 créneau horaire unique.<br>
Pas d’enfants.

> [!NOTE] Exemples
> 1. Alaska : sur la piste de Telaquana (8 janvier 2025, pas d'horaire)<br>
> 2. Anetha & Vel (concert le 24 janvier 2025 à 20h30)

Le cas 2 est en réalité plus proche techniquement d'un récurrent avec un seul créneau, mais ce concept n'est pas compréhensible pour l'usager.

| Propriété | Valeur |
| - | - |
| permalink | /fr/agenda/YYYY/slug/ |
| 1. | /fr/agenda/2025/alaska/ |
| 2. | /fr/agenda/2025/anetha-vel/ |
| file | /content/fr/events/YYYY/MM/DD-slug.html |
| 1. | /content/fr/events/2025/01/08-alaska.html |
| 2. | /content/fr/events/2025/01/24-20-30-anetha-vel.html |
| path | /events/YYYY/MM/DD/slug |
| 1. | /events/2025/01/08-alaska |
| 2. | /events/2025/01/24-20-30-anetha-vel |

> [!WARNING] Slug
> Le slug doit être **unique** dans le scope de l'année. Il ne peut pas être composé que de chiffres.

### Récurrent

Une série de séances sur une journée ou une série de concerts sur plusieurs jours.<br>
Si l'événement chevauche 2 années, on prend l'année de début.<br>
Créneaux horaires multiples.<br>
Pas d’enfants.

Les événements récurrents s'appuient sur des time slots (`Agenda::Event::TimeSlot`), qui génèrent des fichiers statiques.

> [!NOTE] Exemples
> 1+2. Contes à paillettes (14 janvier 2025 à 16h, 20 janvier 2025 à 16h)<br>
> 3+4. Podcast encore heureux (19h et 20h30 20 janvier)

L'événement n'est pas envoyé.<br>
Pour chaque time slot, il faut générer un fichier pour affichage dans la liste.

| Propriété | Valeur |
| - | - |
| permalink | /fr/agenda/YYYY/slug/ |
| 1+2.| /fr/agenda/2025/contes-a-paillettes/ |
| 3+4.| /fr/agenda/2025/encore-heureux/ |
| file | /content/fr/events/YYYY/MM/DD-hh-mm-slug.html |
| 1. | /content/fr/events/2025/01/14-16-00-contes-a-paillettes.html |
| 2. | /content/fr/events/2025/01/20-16-00-contes-a-paillettes.html |
| 3. | /content/fr/events/2025/01/20-19-00-encore-heureux.html |
| 4. | /content/fr/events/2025/01/20-20-30-encore-heureux.html |
| path | /events/YYYY/MM/DD-hh-mm-slug |
| 1+2. | /events/2025/01/14-16-00-contes-a-paillettes |
| 3+4. | /events/2025/01/20-19-00-encore-heureux |

Si on fait un bloc Agenda et qu'on fait un lien vers Contes à paillettes, on pointe vers le 1er timeslot.

> [!WARNING] Slug
> Le slug doit être **unique** dans le scope de l'année. Il ne peut pas être composé que de chiffres.

### Parent

Pas de créneaux.<br>
Enfants (n>1, le cas 1 enfant devrait être traité en événement simple).<br>
Si l'événement chevauche 2 années, on prend l'année de début.<br>
Une journée d’études avec des conférences successives ou un festival de musique.


Afin d'afficher les enfants nichés sous les parents dans les listes, il faut utiliser un objet jour (`Agenda::Event::Day`), qui modélise un jour d'un festival.
Ainsi, l'Arte Concert Festival aura 3 jours, et chaque jour génère un fichier statique afin de permettre à Hugo de retrouver ses petits. A noter que le premier jour génère l'index, ainsi le fichier `/content/fr/events/2025/arte-concert-festival/2025-01-01.html` n'existera pas, au profit du fichier `/content/fr/events/2025/arte-concert-festival/_index.html`.


> [!NOTE] Exemples
> 1. Arte Concert Festival (Du 1er au 3 janvier 2025)<br>
> 2. Rencontres Internationales Paris-Berlin (1 journée le 10 janvier)<br>
> 3. Ciné Club Gaze (plusieurs mois)

| Propriété | Valeur |
| - | - |
| permalink | /fr/agenda/YYYY/slug/ |
| 1. | /fr/agenda/2025/arte-concert-festival/ |
| 2. | /fr/agenda/2025/paris-berlin/ |
| 3. | /fr/agenda/2025/cine-club-gaze/ |
| file | /content/fr/events/YYYY/slug/_index.html |
| 1. | /content/fr/events/2025/arte-concert-festival/_index.html |
| 1. | /content/fr/events/2025/arte-concert-festival/2025-01-02.html |
| 1. | /content/fr/events/2025/arte-concert-festival/2025-01-03.html |
| 2. | /content/fr/events/2025/paris-berlin/_index.html |
| 3. | /content/fr/events/2025/cine-club-gaze/_index.html |
| 3. | /content/fr/events/2025/cine-club-gaze/2020-01-15.html |
| path | /events/YYYY/slug |
| 1. | /events/2025/arte-concert-festival |
| 2. | /events/2025/paris-berlin |
| 3. | /events/2025/cine-club-gaze |

> [!WARNING] Slug
> Le slug doit être **unique** dans le scope de l'année. Il ne peut pas être composé que de chiffres.

### Enfant

Concert pendant un festival.<br>
Un parent, pas d'enfant (pas d’enfant d’enfant).<br>
En général avec un créneau, les cas 0 (pas d’horaire) et n>1 (récurrent) sont possibles.

> [!NOTE] Exemples
> 1+2. Conférences pendant Paris-Berlin<br>
> 3. Concert de Priya Ragu pendant Arte Concert Festival (1er janvier, 20h)<br>
> 4+5. Concerts de Gonzales sur 2 jours différents pendant Arte Concert Festival (le 2 et 3 janvier, 20h)<br>
> 6+7. Séances de ciné à 10 jours d'intervalle, à 20h (Emmanuelle le 5 janvier, Atlantique le 15 janvier)


| Propriété | Valeur |
| - | - |
| permalink | /fr/agenda/YYYY/parent_slug/slug/ |
| 1. | /fr/agenda/2025/paris-berlin/les-fantomes-de-la-liberte/ |
| 2. | /fr/agenda/2025/paris-berlin/resistance-fondamentale/ |
| 3. | /fr/agenda/2025/arte-concert-festival/priya-ragu/ |
| 4+5. | /fr/agenda/2025/arte-concert-festival/gonzales/ |
| 6. | /fr/agenda/2025/cine-club-gaze/emmanuelle/ |
| 7. | /fr/agenda/2025/cine-club-gaze/atlantique/ |
| file | /content/fr/events/YYYY/parent_slug/YYYY-MM-DD-hh-mm-slug.html |
| 1. | /content/fr/events/2025/paris-berlin/2025-01-10-14-00-les-fantomes-de-la-liberte.html |
| 2. | /content/fr/events/2025/paris-berlin/2025-01-10-16-00-resistance-fondamentale.html |
| 3. | /content/fr/events/2025/arte-concert-festival/2025-01-01-20-00-priya-ragu.html |
| 4. | /content/fr/events/2025/arte-concert-festival/2025-01-02-20-00-gonzales.html |
| 5. | /content/fr/events/2025/arte-concert-festival/2025-01-03-20-00-gonzales.html |
| 6. | /content/fr/events/2025/cine-club-gaze/2025/01-05-20-00-emmanuelle.html |
| 7. | /content/fr/events/2025/cine-club-gaze/2025/01-15-20-00-atlantique.html |
| path | /events/YYYY/parent_slug/YYYY-MM-DD-hh-mm-slug |
| 1. | /events/2025/paris-berlin/2025-01-10-14-00-les-fantomes-de-la-liberte |
| 2. | /events/2025/paris-berlin/2025-01-10-16-00-resistance-fondamentale |
| 3. | /events/2025/arte-concert-festival/2025-01-01-20-00-priya-ragu |
| 4. | /events/2025/arte-concert-festival/2025-01-02-20-00-gonzales |
| 5. | /events/2025/arte-concert-festival/2025-01-03/20-00-gonzales |
| 6. | /events/2025/cine-club-gaze/2025-01-05-20-00-emmanuelle |
| 7. | /events/2025/cine-club-gaze/2025-01-15-20-00-atlantique |

> [!WARNING] Slug
> Le slug doit être **unique** dans le scope du jour (`Agenda::Event::Day`) et du parent. <br>
> Il ne peut pas être composé que de chiffres


> [!CAUTION] Attention
> Le système de fichier suivant ignore les parentés, tout est à la racine.

| Propriété | Valeur |
| - | - |
| file | /content/fr/events/YYYY/MM/DD-hh-mm-slug.html |
| 1. | /content/fr/events/2025/01/10-14-00-les-fantomes-de-la-liberte.html |
| 2. | /content/fr/events/2025/01/10-16-00-resistance-fondamentale.html |
| 3. | /content/fr/events/2025/01/01-20-00-priya-ragu.html |
| 4. | /content/fr/events/2025/01/02-20-00-gonzales.html |
| 5. | /content/fr/events/2025/01/03-20-00-gonzales.html |
| 6. | /content/fr/events/2025/01/05-20-00-emmanuelle.html |
| 7. | /content/fr/events/2025/01/15-20-00-atlantique.html |

> [!CAUTION] Attention
> Le système de fichier suivant cause des dissonances de date.
> Un festival à cheval sur 2 ans peut contenir des événements de 2026 dans un dossier 2025.
> Ce système paraît, malgré cela, meilleur que le premier.

| Propriété | Valeur |
| - | - |
| file | /content/fr/events/YYYY/parent_slug/YYYY/MM/DD/hh-mm-slug.html |
| 1. | /content/fr/events/2025/paris-berlin/2025-01-10-14-00-les-fantomes-de-la-liberte.html |
| 2. | /content/fr/events/2025/paris-berlin/2025-01-10-16-00-resistance-fondamentale.html |
| 3. | /content/fr/events/2025/arte-concert-festival/2025-01-01-20-00-priya-ragu.html |
| 4. | /content/fr/events/2025/arte-concert-festival/2025-01-02-20-00-gonzales.html |
| 5. | /content/fr/events/2025/arte-concert-festival/2025-01-03-20-00-gonzales.html |
| 6. | /content/fr/events/2025/cine-club-gaze/2025-01-05-20-00-emmanuelle.html |
| 7. | /content/fr/events/2025/cine-club-gaze/2025-01-15-20-00-atlantique.html |

## Périodes

Pour avoir des sortes de filtres de calendrier, il est nécessaire de générer des pages pour les années et les mois.
Cela ne s'applique que pour les événements, parce qu'il n'y a pas un volume d'expositions suffisant pour que ce soit nécessaire (au Louvre, quelques expos chaque année).

### Année

La page d'une année liste les 12 mois, avec des liens vers chaque mois et des liens ancrés vers chaque jour ayant des événements.

| Propriété | Valeur |
| - | - |
| file | /content/fr/events/YYYY/_index.html |
| | /content/fr/events/2025/_index.html |
| permalink | /fr/agenda/YYYY/ |
| | /fr/agenda/2025/ |
| path | /events/YYYY |
| | /events/2025 |

### Mois

La page d'un mois liste les jours, avec les événements de chaque jour.
Cette page n'est pas paginée, et les jours ont des identifiants permettant de faire un lien avec ancre vers une journée spécifique.

| Propriété | Valeur |
| - | - |
| permalink | /fr/agenda/YYYY/MM/ |
| | /fr/agenda/2025/02/ |
| file | /content/fr/events/YYYY/MM/_index.html |
| | /content/fr/events/2025/02/_index.html |
| path | /events/YYYY/MM |
| | /events/2025/02 |

## Expositions

{{< filetree/container >}}
  {{< filetree/folder name="content" >}}
    {{< filetree/folder name="exhibitions" state="open" >}}
      {{< filetree/folder name="archives" state="closed" >}}{{< /filetree/folder >}}
      {{< filetree/folder name="2025" state="open" >}}
        {{< filetree/file name="pulse.html" >}}
      {{< /filetree/folder >}}
      {{< filetree/file name="_index.html" >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

Une exposition, ouverte tous les jours.<br>
Jour de fin > jour de début.<br>
Pas de créneaux, pas d’enfants.<br>
Rien = tout (ouvert tous les jours).

> [!NOTE] Exemples
> PULSE : Au rythme de la lumière et du son (Du 12 décembre 2025 au 13 juillet 2025)

| Propriété | Valeur |
| - | - | - |
| permalink | /fr/expositions/YYYY/slug/ |
| | /fr/expositions/2025/pulse/ |
| file | /content/fr/exhibitions/YYYY/slug.html |
| | /content/fr/exhibitions/2025/pulse.html |
| path | /exhibitions/YYYY/slug |
| | /exhibitions/2025/pulse |

Aujourd'hui, il n'y a pas de page d'année pour les expositions (`/fr/expositions/2025/` n'amène nulle part).

## Catégories

Une caractérisation des événements et des expositions, par exemple un congrès ou un concert.

## Lieu (pas fait dans la v2)

Un lieu, physique ou virtuel, par exemple l'IUT Bordeaux Montaigne, une chaîne Twitch ou une url de webinaire. Le lieu peuvent avoir des enfants, par exemple des salles à l'IUT, qui sont au sein d'un bâtiment. Un événement peut être connecté à plusieurs lieux, comme c'est le cas pour les événements hybrides.

## Analyse

### Historique

Est-ce vraiment du domaine du site ? Ou est-ce plus haut, avec une publication sur le site ? Par souci de simplicité, on développe les événements au sein des sites Web uniquement. S’ils sont développés aussi au sein des extranets, il est probable que les enjeux fonctionnels soient différents (inscription notamment). À la v1 de l’agenda, produite pour La Criée et l’appel d’offres Villa Médicis, s’ajoute la v2 produite pour la Gaîté Lyrique.

### Exemples

- https://www.college-de-france.fr/fr/agenda
- https://www.college-de-france.fr/fr/agenda/colloque/origines-des-planetes-la-vie
- https://www.college-de-france.fr/fr/agenda/colloque/origines-des-planetes-la-vie/interventions-institutionnelles-mesr-cnrs
- https://www.louvre.fr/expositions-et-evenements/expositions
- https://www.louvre.fr/expositions-et-evenements/evenements-activites
