---
title: Répartition
weight: 2
---

## Principes généraux

Ce qui est traduisible va dans la localisation, ce qui ne l'est pas reste dans l'objet.

### Images à la une

{{< callout type="info" >}}
  Tous les attributs liés aux images à la une sont transférés dans la localisation.
{{< /callout >}}

| Propriété | dans l'objet ? | dans la localisation ? |
|-|-|-|
| featured_image | {{< icon "check-circle" >}} |  {{< icon "check-circle" >}} |
| featured_image_alt | {{< icon "x-circle" >}} |  {{< icon "check-circle" >}} |
| featured_image_credit |  {{< icon "check-circle" >}} |  {{< icon "check-circle" >}} |

Le bénéfice de garder des éléments comme l'image et le crédit dans l'objet est de permettre la mise à jour en une fois.
Le bénéfice de tout localiser est de permettre des choix différents par langue.
Techniquement, il y a un avantage de simplicité à tout grouper.
Comme on ne peut grouper que sur la localisation, on fait ce choix.

### Published

{{< callout type="info" >}}
  Tous les attributs de publication sont transférés dans la localisation.
{{< /callout >}}

| Propriété | dans l'objet ? | dans la localisation ? |
|-|-|-|
| published | {{< icon "x-circle" >}} |  {{< icon "check-circle" >}} |
| published_at | {{< icon "x-circle" >}} |  {{< icon "check-circle" >}} |
| pinned |  {{< icon "check-circle" >}} |  {{< icon "check-circle" >}} |

Pour permettre de publier une localisation et pas une autre, `published` doit être lié à la localisation et pas à l'objet.
La date de publication doit donc suivre.
La propriété `pinned`, qui permet d'épingler un article, pourrait être d'un côté ou de l'autre, selon que l'on veut permettre une gestion éditoriale distincte dans chaque langue.
Là encore, la simplicité technique l'emporte, nous laissons `pinned` avec `published`.

## Objets directs

### Page

| Propriété localisée | Explication |
|-|-|
| breadcrumb_title | Titre traduit |
| featured_image | L'image peut être différente, notamment si elle contient des textes (une affiche par ex.) |
| featured_image_alt | Description alternative traduite |
| featured_image_credit | Crédit traduit |
| header_cta  | Une langue peut avoir un CTA et l'autre pas |
| header_cta_label | Texte traduit |
| header_cta_url | URL traduite |
| header_text | Texte traduit |
| meta_description | Texte traduit |
| migration_identifier | Identifiant utilisé par l'API, présent des 2 côtés |
| published | Publication différenciée |
| published_at | Date différenciée |
| shared_image | L'image de partage contient souvent des textes |
| slug | Le slug dépend du name |
| summary | Texte traduit |
| text | Texte traduit |
| title | Texte traduit |


| Propriété non localisée | Explication |
|-|-|
| bodyclass | Gestion centralisée |
| full_width | Gestion centralisée |
| position | Gestion centralisée |
| migration_identifier | Identifiant utilisé par l'API, présent des 2 côtés |
| parent_id | L'arbre des pages est indépendant des localisations |
| type | Propriété utilisée pour la STI |

#### Pages spéciales

Une grande question concerne les pages spéciales.
Ce sont les pages comme l'accueil, ou l'index des actualités, qui ont un fonctionnement et/ou un rôle particulier dans le site.
Ces objets sont implémentés en utilisant la Single-Table Inheritance (STI) native de Ruby on Rails.
Conceptuellement, rien ne change, ce sont bien les pages qui ont des types spéciaux.

- https://www.martinfowler.com/eaaCatalog/singleTableInheritance.html
- https://api.rubyonrails.org/classes/ActiveRecord/Inheritance.html

Toutefois cela impacte l'implémentation :
- les dépendances partagées avec les localisations
- le `git_path`
- une petite partie du fichier statique


#### Parent spécial manquant

Un cas particulier se produit quand un objet, qu'il s'agisse d'une actualité ou d'une personne, est localisé dans une langue alors que sa page spéciale ne l'est pas.

2 possibilités :
1. soit on considère qu'on ne peut pas afficher l'objet du tout, ce qui ne paraît pas très pertinent
2. soit on considère qu'il faut afficher l'objet, avec un chemin pas trop faux
Concrètement, ça peut donner `/en/equipe/pierre-andre-boissinot`.
Quand la page spéciale Équipe sera traduite, l'url deviendra `/en/team/pierre-andre-boissinot`, et le système de permalinks gèrera la redirection.

### Blog

#### Actualité

`Communication::Website::Post`

| Propriété localisée | Explication |
|-|-|
| featured_image | L'image d'illustration peut être une image avec des textes, comme une affiche par exemple |
| featured_image_alt | Il faut donc traduire les textes en question |
| featured_image_credit | Le crédit peut être traduit ("Photo par...", "Photo by...")
| meta_description | La description de l'actualité est dans la langue |
| migration_identifier | Il faut des identifiants de migration pour tout |
| pinned | La gestion éditoriale peut être différente dans chaque langue |
| published | Il faut pouvoir choisir quand une version est prête. |
| published_at | Pas forcément de cas d'usage au published_at par language, mais l'algo s'appuie sur une combinaison de `published` et `published_at` donc ça paraît une très mauvaise idée de les séparer |
| slug | Le slug dépend du title |
| summary | Le résumé est traduit |
| text | Le texte est traduit |
| title | Le titre est traduit |

| Propriété non localisée | Explication |
|-|-|
| migration_identifier | Un identifiant dans le `Post`, un autre dans la `Localization` |

#### Catégorie

`Communication::Website::Post::Category`

| Propriété localisée | Explication |
|-|-|
| featured_image | L'image d'illustration peut être une image avec des textes, comme une affiche par exemple |
| featured_image_alt | Il faut donc traduire les textes en question |
| featured_image_credit | Le crédit peut être traduit ("Photo par...", "Photo by...")
| meta_description | La description de la catégorie est dans la langue |
| name | Le nom est traduit |
| path | Le chemin est lié au slug |
| slug | Le slug dépend du name |
| summary | Le résumé est traduit |

| Propriété non localisée | Explication |
|-|-|
| is_programs_root | Lié à l'arbre de catégories des formations |
| parent_id | L'arbre de catégories est indépendant des localisations |
| position | La position est lié à l'arbre de catégories |
| program_id | Lié à l'arbre de catégories des formations |


### Menu

`Communication::Website::Menu::Item`

Les menus sont différents dans chaque langue, il ne s'agit pas de localisation. On garde la logique de `language_id`, mais on supprime la notion d'`original_id`.

Comme les menus dans différentes langues partagent un identifiant (par exemple `primary`), on s'appuie sur cet identifiant pour passer d'une langue à l'autre dans un menu.

Les identifiants ne peuvent être définis qu'à la création du menu.

#### Élément

`Communication::Website::Menu::Item`

Les items n'ont pas besoin d'un `language_id` car ils existent dans un menu qui a déjà cette langue définie.

De même, les items de type `blank` et `url` ne sont pas impactés car ne sont liés à aucun objet.

Pour les autres types d'items de menu, ils ont un `about` qui a besoin d'évoluer pour être sur l'objet parent, et non pas une localisation. Il faut effectuer une migration pour bien être connecté à l'original des objets existants.

``` ruby {filename="db/migrate/20240801152544_migrate_communication_website_menu_items_about.rb"}
class MigrateCommunicationWebsiteMenuItemsAbout < ActiveRecord::Migration[7.1]
  def up
    Communication::Website::Menu::Item.where.not(kind: [:blank, :url]).find_each do |menu_item|
      about = menu_item.about
      next if about.nil?
      # Kind like paper can't respond to original_id
      about_id = about.try(:original_id) || about.id
      menu_item.update_column :about_id, about_id
    end
  end

  def down
  end
end
```

Ensuite dans les formulaires, on rajoute le scope `tmp_original` en attendant la fin de la migration.

On centralise la récupération des collections dans le modèle :
``` ruby {filename="app/models/communication/website/menu/item.rb"}
class Communication::Website::Menu::Item < ApplicationRecord
  def self.collection_for(kind, website)
    # TODO L10N : remove every tmp_orginal below
    case kind
    when 'page'
      website.pages.tmp_original
    when 'diploma'
      website.education_diplomas.tmp_original
    when 'program'
      website.education_programs.tmp_original
    when 'category'
      website.post_categories.tmp_original
    when 'post'
      website.posts.tmp_original
    when 'volume'
      website.research_volumes.tmp_original
    when 'paper'
      website.research_papers.tmp_original
    when 'location'
      website.administration_locations.tmp_original
    end
  end
end
```

Dans le fichier statique, on tente de récupérer la localisation de l'objet associé. Si elle n'existe pas, on ne renvoie pas de chemin, l'item de menu ne sera donc pas affiché.

Avant :
``` ruby {filename="app/models/communication/website/menu/item.rb"}
class Communication::Website::Menu::Item < ApplicationRecord
  def static_target
    # ...
    case kind
    when "blank"
      ''
    when "url"
      url
    else
      about.new_permalink_in_website(website).computed_path
    end
  end
end
```

Après :
``` ruby {filename="app/models/communication/website/menu/item.rb"}
class Communication::Website::Menu::Item < ApplicationRecord
  def static_target
    # ...
    case kind
    when "blank"
      ''
    when "url"
      url
    else
      about_l10n = about.localization_for(language)
      if about_l10n.present?
        about_l10n_permalink = about_l10n.new_permalink_in_website(website)
        about_l10n_permalink.computed_path
      else
        nil
      end
    end
  end
end
```
### Agenda

#### Événement

`Agenda::Event`

| Propriété localisée | Explication |
|-|-|
| add_to_calendar_urls | La dénormalisation des URL d'ajout au calendrier dépendent du title |
| featured_image | L'image d'illustration peut être une image avec des textes, comme une affiche par exemple |
| featured_image_alt | Il faut donc traduire les textes en question |
| featured_image_credit | Le crédit peut être traduit ("Photo par...", "Photo by...")
| meta_description | La description de l'actualité est dans la langue |
| migration_identifier | Il faut des identifiants de migration pour tout |
| published | Il faut pouvoir choisir quand une version est prête. |
| published_at | Pas forcément de cas d'usage au published_at par language, mais l'algo s'appuie sur une combinaison de `published` et `published_at` donc ça paraît une très mauvaise idée de les séparer |
| shared_image | L'image de partage peut être une image avec des textes, comme une affiche par exemple |
| slug | Le slug dépend du title |
| subtitle | Le sous-titre est traduit |
| summary | Le résumé est traduit |
| text | Le texte est traduit |
| title | Le titre est traduit |

| Propriété non localisée | Explication |
|-|-|
| from_day | Les dates de l'évènement sont indépendantes des localisations |
| from_hour | Idem `from_day` |
| migration_identifier | Un identifiant dans le `Event`, un autre dans la `Localization` |
| parent_id | L'arbre des évènements est indépendant des localisations |
| time_zone | Idem `from_day` |
| to_day | Idem `from_day` |
| to_hour | Idem `from_day` |

#### Catégorie

`Agenda::Category`

| Propriété localisée | Explication |
|-|-|
| featured_image | L'image d'illustration peut être une image avec des textes, comme une affiche par exemple |
| featured_image_alt | Il faut donc traduire les textes en question |
| featured_image_credit | Le crédit peut être traduit ("Photo par...", "Photo by...")
| meta_description | La description de la catégorie est dans la langue |
| name | Le nom est traduit |
| path | Le chemin est lié au slug |
| slug | Le slug dépend du name |
| summary | Le résumé est traduit |

| Propriété non localisée | Explication |
|-|-|
| is_programs_root | Lié à l'arbre de catégories des formations |
| parent_id | L'arbre de catégories est indépendant des localisations |
| position | La position est lié à l'arbre de catégories |
| program_id | Lié à l'arbre de catégories des formations |

### Portfolio

#### Projet

`Portfolio::Project`

| Propriété localisée | Explication |
|-|-|
| featured_image | L'image d'illustration peut être une image avec des textes, comme une affiche par exemple |
| featured_image_alt | Il faut donc traduire les textes en question |
| featured_image_credit | Le crédit peut être traduit ("Photo par...", "Photo by...")
| meta_description | La description de l'actualité est dans la langue |
| migration_identifier | Il faut des identifiants de migration pour tout |
| published | Il faut pouvoir choisir quand une version est prête. |
| published_at | Pas forcément de cas d'usage au published_at par language, mais l'algo s'appuie sur une combinaison de `published` et `published_at` donc ça paraît une très mauvaise idée de les séparer |
| shared_image | L'image de partage peut être une image avec des textes, comme une affiche par exemple |
| slug | Le slug dépend du title |
| summary | Le résumé est traduit |
| title | Le titre est traduit |

| Propriété non localisée | Explication |
|-|-|
| year | L'année du projet est indépendant des localisations |

#### Catégorie

`Portfolio::Category`

| Propriété localisée | Explication |
|-|-|
| featured_image | L'image d'illustration peut être une image avec des textes, comme une affiche par exemple |
| featured_image_alt | Il faut donc traduire les textes en question |
| featured_image_credit | Le crédit peut être traduit ("Photo par...", "Photo by...")
| meta_description | La description de la catégorie est dans la langue |
| name | Le nom est traduit |
| path | Le chemin est lié au slug |
| slug | Le slug dépend du name |
| summary | Le résumé est traduit |

| Propriété non localisée | Explication |
|-|-|
| is_programs_root | Lié à l'arbre de catégories des formations |
| is_taxonomy | Le système de facettes est indépendant des localisations |
| parent_id | L'arbre de catégories est indépendant des localisations |
| position | La position est lié à l'arbre de catégories |
| program_id | Lié à l'arbre de catégories des formations |

## Objets indirects

### Organisation

`University::Organization`

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

#### Catégorie

`University::Organization::Category`

| Propriété localisée | Explication |
|-|-|
| name | Le nom est traduit |
| slug | Le slug dépend du name |

| Propriété non localisée | Explication |
|-|-|
| parent_id | L'arbre de catégories est indépendant des localisations |
| position | La position est lié à l'arbre de catégories |

### Person

`University::Organization`

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

#### Facettes

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


#### Catégorie

`University::Organization::Category`

| Propriété localisée | Explication |
|-|-|
| name | Le nom est traduit |
| slug | Le slug dépend du name |

| Propriété non localisée | Explication |
|-|-|
| parent_id | L'arbre de catégories est indépendant des localisations |
| position | La position est liée à l'arbre de catégories |

