---
title: Objets indirects
weight: 2
---

## Organization

| Propriété localisée | Explication |
|-|-|
| address_additional | Cette propriété est très versatile, mais on peut imaginer traduire "Bureau 508" |
| address_name | "Head office", "Siège social"... |
| linkedin | Les urls sont différentes en fonction des langues |
| long_name | "Groupe d'experts intergouvernemental sur l'évolution du climat", "Intergovernmental Panel on Climate Change" |
| mastodon | Les comptes peuvent être différents pour chaque langue |
| meta_description | La description de page est dans la langue |
| name | "GIEC", "IPCC" |
| summary | Le résumé est dans la langue |
| slug | Le slug dépend du nom |
| text | La présentation de l'organisation est localisée|
| twitter | Les comptes peuvent être différents pour chaque langue |
| url | Les urls peuvent être distinctes, qu'il s'agissent de domaines différents ou d'un /fr /en |
| logo | Le logo peut être complètement différent (si le nom l'est) ou juste avoir des variantes locales |
| logo_on_dark_background | Idem logo |

| Propriété non localisée | Explication |
|-|-|
| active | Pas évident du tout : faut-il pouvoir désactiver l'orga entière ? Ou bien les locas ? |
| address | L'adresse est identique quelle que soit la langue |
| city | La ville est identique quelle que soit la langue |
| country | Le pays est un code (FR, IT), donc neutre linguistiquement |
| email | Pas évident du tout : pourquoi un mail unique ? |
| kind | Le type (asso, organisation publique) ne dépend pas de la langue, même si la traduction peut varier :ASBL en Belgique, association loi 1901 en France, non-profit au UK... |
| latitude | La latitude ne change pas en fonction de la langue |
| longitude | La longitude ne change pas en fonction de la langue |
| nic | Cette propriété est probablement inutilisée, c'est un ajout au SIREN pour composer le SIRET |
| phone | Pas évident du tout : il pourrait y avoir un numéro par langue |
| siren | Siren et NIC sont trop français, il faudrait un nom de propriété international. Quoi qu'il en soit, ça ne change pas en fonction de la langue. |
| zipcode | La traduction ne change rien au code postal |

### + Category

`Organization::Category`

| Propriété localisée | Explication |
|-|-|
| name | Le nom est traduit |
| slug | Le slug dépend du name |

| Propriété non localisée | Explication |
|-|-|
| parent_id | L'arbre de catégories est indépendant des localisations |
| position | La position est lié à l'arbre de catégories |

## Person

Les personnes ont des facettes : `Administrator`, `Author`, `Researcher`, `Teacher`.
Les facettes sont activées par des booléens, comme `is_author` (attention il y a des subtilités).
Ces 4 facettes permettent de gérer les 5 pages différentes possibles pour une même personne dans Hugo.
Les facettes sont devenues des classes qui héritent de la localisation et plus de la personne elle-même,
parce qu'elles ont un slug qui dépend du nom (traduit).
Dans les dépendances des pages spéciales, comme la page qui liste les administrateurs, il faut lister les administrateurs.
Attention, prendre toutes les personnes qui ont un rôle d'administration amènerait à une situation fausse :
une personne pourrait être administratrice dans un site d'une formation, alors qu'elle n'a pas de rôle administratif dans la formation, mais dans l'école.
Il faut donc se limiter au contexte du site.

``` ruby {filename="app/models/communication/website/page/administrator.rb"}
def dependencies_administrators
  University::Person::Localization::Administrator.where(
    about_id: website.administrators.pluck(:id),
    language_id: website.active_language_ids
  )
end
```
On récupère les facettes de localisations `Administrator` des personnes qui ont un rôle administratif pour ce site.
La limite aux langues actives évite d'envoyer des langues non utilisées dans le site.

| Propriété localisée | Explication |
|-|-|
| biography | La biographie doit être traduite |
| first_name | Le prénom semble être non traduisible, mais... Sacha en russe, c'est Саша |
| last_name | Idem prénom |
| linkedin | Les urls sont différentes par langue |
| mastodon | Il peut y avoir un compte différent par langue |
| meta_description | La description doit être traduite |
| name | Dénormalisation de first name + last name |
| picture_credit | Crédit traduit (par, by...) |
| slug | dépend du name |
| summary | Résumé traduit |
| twitter | Il peut y avoir un compte par langue|
| url | Les urls sont différentes par langue, soit /fr /en, soit des sous-domaines, soit des extensions .fr ou .co.uk |

| Propriété non localisée | Explication |
|-|-|
| address | L'adresse est identique quelle que soit la langue |
| address_visibility | |
| birthdate | La date de naissance ne dépend pas de la langue |
| city | La ville est identique quelle que soit la langue |
| country | Le pays est un code (FR, IT), donc neutre linguistiquement |
| email | Pas évident du tout : pourquoi un mail unique ? C'est probablement le cas le plus commun |
| email_visibility | Les réglages de visibilité ne changent pas en fonction des langues |
| gender | Le genre ne dépend pas des langues |
| habilitation | Les états de la personne ne dépendent pas des langues |
| is_administration | Les états de la personne ne dépendent pas des langues |
| is_alumnus | Les états de la personne ne dépendent pas des langues |
| is_author | Les états de la personne ne dépendent pas des langues |
| is_researcher | Les états de la personne ne dépendent pas des langues |
| is_teacher | Les états de la personne ne dépendent pas des langues |
| linkedin_visibility | Les réglages de visibilité ne changent pas en fonction des langues |
| mastodon_visibility | Les réglages de visibilité ne changent pas en fonction des langues |
| tenure | Les états de la personne ne dépendent pas des langues |
| twitter_visibility | Les réglages de visibilité ne changent pas en fonction des langues |
| zipcode | Les états de la personne ne dépendent pas des langues |

Les personnes ont 3 tables liées :
- `university_person_experiences`
- `university_person_involvements`
- `university_roles`

Ces 3 tables n'avaient pas de gestion des langues avant l'énorme [PR 2025](https://github.com/osunyorg/admin/pull/2025).
Il n'y a donc rien à migrer.

### + Category

`Person::Category`

| Propriété localisée | Explication |
|-|-|
| name | Le nom est traduit |
| slug | Le slug dépend du name |

| Propriété non localisée | Explication |
|-|-|
| parent_id | L'arbre de catégories est indépendant des localisations |
| position | La position est lié à l'arbre de catégories |

