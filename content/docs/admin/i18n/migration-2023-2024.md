---
title: Migration 2023-2024
weight: 2
---

## Méthodologie 


Une pull request synthétise la migration d'un objet direct :
https://github.com/osunyorg/admin/pull/2123


La méthode est la suivante : 
1. Modifier la base de données
2. Modifier les modèles
3. Modifier le controleur
4. Modifier les vues
5. Modifier les locales

Les routes ne bougent pas !

### 1. Base de donnnées

#### Migrer l'objet principal

Toutes les informations localisées sont supprimées de l'organisation elle-même, et déplacées dans la localisation.
Pour gérer la migration, cela se fait en 2 passes, d'abord le déplacement, puis la suppression.
La partie documentée ici est la première passe.

``` ruby {filename="db/migrate/20240729140421_create_communication_agenda_event_localizations.rb"}
class CreateCommunicationAgendaEventLocalizations < ActiveRecord::Migration[7.1]
  def up
    change_column_null :communication_website_agenda_events, :language_id, true

    create_table :communication_website_agenda_event_localizations, id: :uuid do |t|
      t.jsonb :add_to_calendar_urls
      t.string :featured_image_alt
      t.text :featured_image_credit
      t.string :meta_description
      t.string :migration_identifier
      t.boolean :published, default: false
      t.datetime :published_at
      t.string :slug
      t.string :subtitle
      t.text :summary
      t.text :text # No text yet
      t.string :title

      t.references :about, foreign_key: { to_table: :communication_website_agenda_events }, type: :uuid
      t.references :language, foreign_key: true, type: :uuid
      t.references :communication_website, foreign_key: true, type: :uuid
      t.references :university, foreign_key: true, type: :uuid

      t.timestamps
    end

    Communication::Website::Agenda::Event.find_each do |event|
      puts "Migration event #{event.id}"

      about_id = event.original_id || event.id

      l10n = Communication::Website::Agenda::Event::Localization.create(
        add_to_calendar_urls: event.add_to_calendar_urls,
        featured_image_alt: event.featured_image_alt,
        featured_image_credit: event.featured_image_credit,
        meta_description: event.meta_description,
        migration_identifier: event.migration_identifier,
        published: event.published,
        published_at: event.updated_at, # No published_at yet
        slug: event.slug,
        subtitle: event.subtitle,
        summary: event.summary,
        title: event.title,
        about_id: about_id,

        language_id: event.language_id,
        communication_website_id: event.communication_website_id,
        university_id: event.university_id,
        created_at: event.created_at
      )

      event.translate_contents!(l10n)
      event.translate_attachment(l10n, :featured_image)
      event.translate_other_attachments(l10n)

      event.permalinks.each do |permalink|
        new_permalink = permalink.dup
        new_permalink.about = l10n
        new_permalink.save
      end

      l10n.save
    end
  end

  def down
    drop_table :communication_website_agenda_event_localizations
  end
end
```

#### Migrer les catégories liées

TODO

### 2. Modèles

#### Créer le modèle de localisation

``` ruby {filename="app/models/communication/website/agenda/event/localization.rb"}
class Communication::Website::Agenda::Event::Localization < ApplicationRecord
  ...
end
```

D'abord on crée le fichier du modèle.

``` ruby {filename="app/models/communication/website/agenda/event/localization.rb"}
class Communication::Website::Agenda::Event::Localization < ApplicationRecord
  include AsLocalization
  include Contentful
  include Initials
  include Permalinkable
  include Sanitizable
  include Shareable
  include WithAccessibility
  include WithBlobs
  include WithCal
  include WithFeaturedImage
  include WithGitFiles
  include WithPublication
  include WithUniversity
  ...
end
```
Puis on ajoute les traits nécessaires :
- comportements communs des localisations
- blocks
- initiales pour l'affichage dans les listes si pas de visuel
- permaliens et alias
- sécurité par contrôle des entrées
- visuel de partage
- contrôle d'accessibilité
- gestion des blobs attachés
- trait spécifique pour la création des liens d'ajout au calendrier
- visuel principal
- fichiers Git
- système de publication
- multitenant

``` ruby {filename="app/models/communication/website/agenda/event/localization.rb"}
class Communication::Website::Agenda::Event::Localization < ApplicationRecord
  ...

  belongs_to :website,
              class_name: 'Communication::Website',
              foreign_key: :communication_website_id

  alias :event :about

  delegate  :archive?, 
            :from_day, :from_hour,
            :to_day, :to_hour,
            :time_zone,
            to: :event

  has_summernote :text
  
  validates :title, presence: true

  before_validation :set_communication_website_id

end
```
Ensuite on ajoute les relations, les délégations et les validations :
- lien au site Web
- alias pour clarifier
- délégations liées aux méthodes laissées dans le modèle `Event`
- champ de texte pour le futur (Agenda Gaîté Lyrique)
- validation du titre dans la l10n
- récupération de l'id du site Web 


``` ruby {filename="app/models/communication/website/agenda/event/localization.rb"}
class Communication::Website::Agenda::Event::Localization < ApplicationRecord
  ...

  def git_path(website)
    return unless website.id == communication_website_id && published && published_at
    git_path_content_prefix(website) + git_path_relative
  end

  def git_path_relative
    path = "events/"
    path += "archives/#{from_day.year}/" if archive?
    path += "#{from_day.strftime "%Y-%m-%d"}-#{slug}.html"
    path
  end

  def template_static
    "admin/communication/websites/agenda/events/static"
  end

  def static_path
    "#{published_at.year}/#{published_at.strftime "%Y-%m-%d"}-#{slug}"
  end

  def dependencies
    active_storage_blobs +
    contents_dependencies
  end

  def categories
    about.categories.ordered.map { |category| category.localization_for(language) }.compact
  end

  def to_s
    "#{title}"
  end

  ...
end
```
On définit les méthodes publiques :
- le chemin absolu du fichier git dans un site Web
- le chemin relatif du fichier git
- le chemin du template static
- le chemin static (ça pose encore question, ça ne devrait pas être là mais dans le permalink)
- les dépendances
- les références (là il n'y en a pas, parce qu'elles sont gérées dans l'événement)
- une méthode d'utilité pour récupérer les catégories localisées (si elles existent)

``` ruby {filename="app/models/communication/website/agenda/event/localization.rb"}
class Communication::Website::Agenda::Event::Localization < ApplicationRecord
  ...

  protected

  def check_accessibility
    accessibility_merge_array blocks
  end

  def slug_unavailable?(slug)
    self.class.unscoped
              .where(communication_website_id: self.communication_website_id, language_id: language_id, slug: slug)
              .where.not(id: self.id)
              .exists?
  end

  def set_communication_website_id
    self.communication_website_id ||= about.communication_website_id
  end

  def explicit_blob_ids
    super.concat [
      featured_image&.blob_id,
      shared_image&.blob_id
    ]
  end

  def localize_other_attachments(localization)
    localize_attachment(localization, :shared_image) if shared_image.attached?
  end
end
```
Enfin, les méthodes protégées :
- la vérification d'accessibilité
- la restriction du slug
- la définition du website_id
- la liste des blobs
- la localisation d'éléments supplémentaires (mais on peut se demander pourquoi l'image de partage n'est pas standard)

#### Modifier l'objet principal


#### Mapper le permalink

Il faut ajouter la localisation au tableau MAPPING pour faire le lien avec la classe en charge des permaliens.

``` ruby {filename="app/models/communication/website/permalink/with_mapping.rb"}
...
      "Communication::Website::Agenda::Event::Localization" => Communication::Website::Permalink::Agenda::Event,
...
```


### 3. Controleur
### 4. Vues
### 5. Locales


## Objets indirects

### Organization

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

#### + Category

`Organization::Category`

| Propriété localisée | Explication |
|-|-|
| name | Le nom est traduit |
| slug | Le slug dépend du name |

| Propriété non localisée | Explication |
|-|-|
| parent_id | L'arbre de catégories est indépendant des localisations |
| position | La position est lié à l'arbre de catégories |

### Person

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

#### + Category

`Person::Category`

| Propriété localisée | Explication |
|-|-|
| name | Le nom est traduit |
| slug | Le slug dépend du name |

| Propriété non localisée | Explication |
|-|-|
| parent_id | L'arbre de catégories est indépendant des localisations |
| position | La position est lié à l'arbre de catégories |


## Objets directs

### Post

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

#### + Category

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


### Menu 

`Communication::Website::Menu::Item`

Les menus sont différents dans chaque langue, il ne s'agit pas de localisation. On garde la logique de `language_id`, mais on supprime la notion d'`original_id`.

Comme les menus dans différentes langues partagent un identifiant (par exemple `primary`), on s'appuie sur cet identifiant pour passer d'une langue à l'autre dans un menu. 

Les identifiants ne peuvent être définis qu'à la création du menu.

#### + Item

`Communication::Website::Menu::Item`

TODO @seb

### Événements
