---
title: Missions
---

Les personnes ont différentes missions : 
- les enseignants enseignent dans des formations
- certaines personnes remplissent une mission administrative au sein d'une formation
- le personnel administratif de l'Université remplissent une mission administrative au sein de l'Université, d'une école, d'un laboratoire...

## Involvement

```
University::Person::Involvement
```

L'objet involvement sert à faire le lien entre une personne et une cible (target), qui peut être :
- une formation (pour les enseignants)
- un rôle (pour les autres)

Dans l'involvement, on a une description permettant de décrire ce que fait précisément la personne, par exemple :
- ce qu'elle enseigne (Expression et communication)
- ce qu'elle fait dans un service (Chef de service)

## Roles

```
University::Role
```

L'objet role permet de définir une mission, dont le contexte est indiqué par la propriété `target`: 
- Cheffe de département, (target -> une formation)
- Direction (target -> une école)
- Vice-présidence (target -> Université)

## Exemples

Soient :
- `mmi_program` : l'objet `Education::Program` représentant le BUT MMI
- `teacher` : l'objet `University::Person` représentant un enseignant
- `director` : l'objet `University::Person` représentant la cheffe de département
- `program_manager` : l'objet `University::Person` représentant le directeur des études
- `secretary` : l'objet `University::Person` représentant le secrétaire

Pour l'enseignant on crée un objet `University::Person::Involvement`:
- target: `mmi_program`
- person: `teacher`
- kind: teacher

Pour la cheffe de département on crée :
- Un objet `University::Role`, qu'on nomme `director_role`
  - target: `mmi_program`
  - description: "Cheffe de département"
- Un objet `University::Person::Involvement`
  - target: `director_role`
  - person: `director`
  - kind: administrator

Pour le directeur des études on crée :
- Un objet `University::Role`, qu'on nomme `program_manager_role`
  - target: `mmi_program`
  - description: "Directeur des études"
- Un objet `University::Person::Involvement`
  - target: `program_manager_role`
  - person: `program_manager`
  - kind: administrator

Pour le secrétaire on crée :
- Un objet `University::Role`, qu'on nomme `secretary_role`
  - target: `mmi_program`
  - description: "Secrétaire"
- Un objet `University::Person::Involvement`
  - target: `secretary_role`
  - person: `secretary`
  - kind: administrator
