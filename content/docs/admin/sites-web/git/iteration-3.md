---
title: Itération 3
---

20 juillet 2026

Depuis quelque temps, il arrive que la synchronisation de certains sites soient bloquée. Le repository du site de Rennes Ville et Métropole semble être le + exposé à ce problème.

Côté GitHub, cela donnait l'erreur suivante : `Octokit::UnprocessableEntity: POST https://api.github.com/repos/osunyorg/rennes-metropole/git/trees: 422 - GitRPC::BadObjectState`

## Workaround

La première piste a été que les commits étaient trop larges. A noter qu'on n'avait pas encore de repositories GitLab à ce moment-là.

Pour résoudre le problème, on a réduit la valeur de la constante `Git::Providers::Github::COMMIT_BATCH_SIZE` de 100 à 20.

Cela a débloqué 90 % des cas, mais parfois, on se retrouvait avec un blocage. On repassait donc la synchronisation manuellement en console en abaissant encore cette constante.

## Le cas GitLab

Lors de la migration des sites de Rennes vers GitLab, la synchronisation s'est de nouveau retrouvée bloquée pour 2 sites en particulier : le site Métropole et le site « Déchets et Propreté ».

L'erreur est plus spécifique :
- `Gitlab::Error::BadRequest: Server responded with code 400, message: A file with this name doesn't exist.`
- `Gitlab::Error::BadRequest: Server responded with code 400, message: A file with this name already exists.`

Leur point commun : la fonctionnalité Agenda. Pour le contexte, les événements viennent d'une synchronisation à partir d'OpenAgenda. Ecedi a développé un automate qui récupère les événements OpenAgenda, et les synchronise vers osuny via l'API. Cependant, la gestion des créneaux horaires se fait de cette façon.

A un moment T, on peut avoir un événement décrivable comme ceci :
```
Événement A le 1er août
- Créneau horaire n°1 : 11h (fichier : content/fr/events/2026/08/01-11-00-nom-evenement)
- Créneau horaire n°2 : 12h (fichier : content/fr/events/2026/08/01-12-00-nom-evenement)
```

Puis à une synchronisation ultérieure :
```
Événement A le 1er août
- Créneau horaire n°1 : 12h (fichier : content/fr/events/2026/08/01-12-00-nom-evenement)
- Créneau horaire n°2 : 13h (fichier : content/fr/events/2026/08/01-13-00-nom-evenement)
```

Or, dans le batch de modifications, cela donne quelque chose comme

```
[
  {
    action: "move",
    previous_path: "content/fr/events/2026/08/01-11-00-nom-evenement",
    file_path: "content/fr/events/2026/08/01-12-00-nom-evenement"
  },
  {
    action: "update",
    file_path: "content/fr/events/2026/08/01-12-00-nom-evenement",
    content: "..."
  },
  {
    action: "move",
    previous_path: "content/fr/events/2026/08/01-12-00-nom-evenement",
    file_path: "content/fr/events/2026/08/01-13-00-nom-evenement"
  },
  {
    action: "update",
    file_path: "content/fr/events/2026/08/01-13-00-nom-evenement",
    content: "..."
  },
]
```

Or, la 1re action veut déplacer le 1er créneau de 11h à 12h, mais cela va bloquer car le fichier existe déjà (créneau n°2).

## Solution

Afin d'éviter les incohérences, on effectue 2 opérations sur le batch de modifications.

1. On commence par mettre toutes les opérations de suppression au début du batch.
2. On effectue une passe sur tous les éléments, en stockant l'état du fichier (existant ou supprimé) à chaque path.
  - Si on supprime un fichier déjà supprimé, l'action ne sert à rien, on la supprime
  - Si on crée un fichier déjà existant, il faut transformer l'action en "modification"
  - Si on modifie un fichier supprimé, il faut transformer l'action en "création"

Ainsi, on résout le problème de race condition.

Prenons pour exemple ce batch :

```ruby
[
  { action: 'create', file_path: 'path/file/1', content: 'content' },
  { action: 'update', file_path: 'path/file/2', content: 'content' },
  { action: 'delete', file_path: 'path/file/3', content: 'content' },
  { action: 'delete', file_path: 'path/file/4', content: 'content' },
  { action: 'update', file_path: 'path/file/5', content: 'content' },
  { action: 'delete', file_path: 'path/file/5', content: 'content' },
  { action: 'create', file_path: 'path/file/6', content: 'content' },
  { action: 'delete', file_path: 'path/file/3', content: 'content' },
  { action: 'create', file_path: 'path/file/7', content: 'content' }
]
```

Après la 1re opération, on arrive à :

```ruby
[
  { action: 'delete', file_path: 'path/file/3', content: 'content' },
  { action: 'delete', file_path: 'path/file/4', content: 'content' },
  { action: 'delete', file_path: 'path/file/5', content: 'content' },
  { action: 'delete', file_path: 'path/file/3', content: 'content' },
  { action: 'create', file_path: 'path/file/1', content: 'content' },
  { action: 'update', file_path: 'path/file/2', content: 'content' },
  { action: 'update', file_path: 'path/file/5', content: 'content' },
  { action: 'create', file_path: 'path/file/6', content: 'content' },
  { action: 'create', file_path: 'path/file/7', content: 'content' }
]
```

Après la 2nde opération, on arrive à :

```ruby
[
  { action: 'delete', file_path: 'path/file/3', content: 'content' },
  { action: 'delete', file_path: 'path/file/4', content: 'content' },
  { action: 'delete', file_path: 'path/file/5', content: 'content' },
  # Suppresion du doublon de suppression de "path/file/3"
  { action: 'create', file_path: 'path/file/1', content: 'content' },
  { action: 'update', file_path: 'path/file/2', content: 'content' },
  { action: 'create', file_path: 'path/file/5', content: 'content' }, # La modification devient une création (supprimé avant)
  { action: 'create', file_path: 'path/file/6', content: 'content' },
  { action: 'create', file_path: 'path/file/7', content: 'content' }
]
```

