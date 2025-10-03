---
title: Utilisateurs et utilisatrices
weight: 90
---
## Profil

Les utilisateurs peuvent gérer leur propre compte sur la page 
https://[instance].osuny.org/admin/[langue]/profile


## GDPR

Pour se conformer aux obligatuons légales les comptes utilisateurs non actifs sont supprimés automatiquement après une période d'inactivité de 3 ans. 

1 mois avant la suppression automatique ils reçoivent un email pour les prévenir de la suppression à venir de leur compte et les inviter à revtourner se connecter. En cas d'inaction le compte est ensuite supprimé.

## Model

- university:references
- first_name:string
- last_name:string
- role:integer (enum: superadmin, admin, visitor)
