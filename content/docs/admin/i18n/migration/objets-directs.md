---
title: Objets directs
weight: 3
---

## Post

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

### + Category

`Post::Category`

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

## Page

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

### Pages spéciales

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


### Parent spécial manquant

Un cas particulier se produit quand un objet, qu'il s'agisse d'une actualité ou d'une personne, est localisé dans une langue alors que sa page spéciale ne l'est pas.

2 possibilités :
1. soit on considère qu'on ne peut pas afficher l'objet du tout, ce qui ne paraît pas très pertinent
2. soit on considère qu'il faut afficher l'objet, avec un chemin pas trop faux
Concrètement, ça peut donner `/en/equipe/pierre-andre-boissinot`.
Quand la page spéciale Équipe sera traduite, l'url deviendra `/en/team/pierre-andre-boissinot`, et le système de permalinks gèrera la redirection.


## Menu

`Communication::Website::Menu::Item`

Les menus sont différents dans chaque langue, il ne s'agit pas de localisation. On garde la logique de `language_id`, mais on supprime la notion d'`original_id`.

Comme les menus dans différentes langues partagent un identifiant (par exemple `primary`), on s'appuie sur cet identifiant pour passer d'une langue à l'autre dans un menu.

Les identifiants ne peuvent être définis qu'à la création du menu.

### + Item

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

## Événements

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

### + Category

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

## Projets

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

### + Category

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
