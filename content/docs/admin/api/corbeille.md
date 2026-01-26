---
title: API et corbeille
---

3 pistes possibles
1. pas de corbeille
2. magie de la restauration
3. strict mode

## Pas de corbeille

- `really_destroy!` au lieu de `destroy`
- gestion de la suppression réelle des time_slots via une méthode `really_destroy_timeslots`

Cette piste est implémentée par [la PR 3685](https://github.com/osunyorg/admin/pull/3685). 

----

## Magie de la restauration

- quand on crée
  - si migration_identifier existe déjà (non supprimé), 422
  - si migration_identifier existe déjà (soft-deleted)
    - soit on restaure et update
    - soit on really_destroy! et on crée
- quand on update,
  - si migration_identifier existe déjà (supprimé ou non), et que c'est le mauvais objet, 422
  - si migration_identifier existe déjà (non supprimé) et que c'est le bon objet, update
  - si migration_identifier existe déjà (soft-deleted) et que c'est soft-deleted, restaurer et update
- quand on upsert,
  - si migration_identifier existe déjà (non supprimé), update
  - si migration_identifier existe déjà (soft-deleted) et que c'est soft-deleted, restaurer et update
  - si migration_identifier n'existe pas, on crée

- Gérer l'unicité des migration_identifier
- Gérer la restauration quand on trouve un migration_identifier dans la corbeille

----

## Strict mode

- pas de restauration magique
- Si on crée avec un objet en corbeille avec le même migration_identifier, 422
- Si on update un objet en corbeille avec le même migration_identifier, 422
- Si on upsert avec un objet en corbeille avec le même migration_identifier, 422