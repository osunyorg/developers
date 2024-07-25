---
title: Méthode 2024
weight: 1
---
{{< callout type="info" >}}
Méthode appliquée à partir de juillet 2024.
{{< /callout >}}

## Principe

L'ancienne méthode s'appuie sur des cercles d'objets, dont l'un est l'original et l'autre les traductions.
Ainsi, un objet qui existe en 3 langues est en fait une collection de 3 objets, dont l'un des 3 est l'original.
Cela pose des problèmes pour gérer les liens entre objets, parce qu'il faut créer des liens entre objets du même espace linguistique.
Quand on supprime un lien, faut-il le supprimer partout ? 
Quand on crée une nouvelle traduction, faut-il créer automatiquement les traductions des objets liés ?

La nouvelle méthode sépare les contenus traduits des objets eux-mêmes.
Un objet `Localization` est ajouté pour chaque objet, nommé `about`.
Ainsi, une organisation a des localisations.

Les blocs eux-mêmes sont attachés aux localisations, et plus aux objets.
On a donc un ensemble d'objets reliés entre eux, comme peuvent l'être un article et son auteur, et pour chacun de ces objets, des localisations.

Certaines données ne sont pas localisables, comme par exemple les coordonnées géographiques d'une organisation, ou sa ville.
Le fait de traduire une organisation ne change pas son siège social.
Il faut donc définir, pour chaque objet, ce qui se traduit ou pas.
C'est en réalité assez subtil, donc ce sera documenté ci-dessous.

## Implémentation

### Routes

Toutes les routes de l'admin commencent maintenant par une langue :
```
/admin/fr/university/organizations
```

Donc les routes qui géraient les langues précédemment disparaissent (l'exemple indique les 2 routes supprimées):
``` ruby {filename="config/routes/admin/university.rb"}
  resources :organizations do
    collection do
      resources :categories, controller: 'organizations/categories', as: 'organization_categories' do
        member do
          get "/translations/:lang" => "organizations/categories#in_language", as: :show_in_language
        end
      end
    end
    member do
      get "/translations/:lang" => "organizations#in_language", as: :show_in_language
    end
  end
```

Petite particularité sur les routes gérant les blocs et headings, l'objet concerné est maintenant la localisation
```
/admin/fr/communication/blocks/new?about_id=cb548aaf-f11e-4cbc-bb1b-d9b52b33493c&about_type=University%3A%3AOrganization%3A%3ALocalization
```
Ce n'est pas un choix évident, on pourrait faire avec l'organisation directement plutôt que la localisation de l'organisation, puisqu'on dispose de la langue globalement. Cela crée un effet de bord sans gravité (si changement de langue au moment de l'ajout d'un bloc), mais comme tout sera refondu pour le futur éditeur de contenu, on ne s'en préoccupe pas plus.

### Models

Les modèles localisés utilisent le concern `Localizable`, qui fournit toutes les fonctionnalités nécessaires (il est d'ailleurs un peu trop gros).

```ruby {filename="app/models/university/organization.rb"}
class University::Organization < ApplicationRecord
  include Localizable
```

Les localisations utilisent le concern `AsLocalization`, qui fournit les fonctionnalités communes à toutes les localisations.

```ruby {filename="app/models/university/organization/localization.rb"}
class University::Organization::Localization < ApplicationRecord
  include AsLocalization
```

Il faut rajouter une entrée dans le mapping des permalinks pour la localisation, en reprenant le mapping de l'objet initial, qui lui, ne sera plus envoyé sur Git.

``` ruby {filename="app/models/communication/website/permalink/with_mapping.rb"}
module Communication::Website::Permalink::WithMapping
  extend ActiveSupport::Concern

  included do
    MAPPING = {
      # ...
      "University::Organization::Localization" => Communication::Website::Permalink::Organization,
      # ...
    }
  # ...
end
```

#### Objet indirect

L'exemple suivant est la `Person`, mais cela fonctionne à l'identique pour les autres objets indirects (`Organization`, `Program`...).

``` ruby {filename="app/models/university/person.rb"}
class University::Person < ApplicationRecord
  include AsIndirectObject
  include Localizable
```

Le module `AsIndirectObject` gère les dépendances, les références et les connexions.

Le module `Localizable` gère le lien aux localisations (dans ce cas, c'est le modèle `University::Person::Localization`).

{{< callout type="warning" >}}
Il n'y a plus les modules...
- `WithGitFiles` parce que l'objet lui-même n'est pas envoyé sur Git, ce sont ses localisations qui sont envoyées.  
- `Permalinkable` parce que le path et le slug dépendent de la langue.
- `Contentful` parce que l'objet lui-même n'a plus de blocs, ce sont ses localisations qui en ont.
- `Backlinkable` parce que ce sont les localisations, plus précisément les blocks liés aux localisations, qui créent les backlinks.
{{</ callout >}}

``` ruby {filename="app/models/university/person/localization.rb"}
class University::Person::Localization < ApplicationRecord
  include AsLocalization
  include Backlinkable
  include Contentful
  include Permalinkable
  include WithGitFiles
```

Le module `AsLocalization` gère le lien à la langue, à l'objet et à l'université concernés.  Il fournit aussi les dépendances.
Le module `Backlinkable` fournit les liens inverses, cf paragraphe ci-dessous.
Le module `Contentful` ajoute les blocs.
Le module `Permalinkable` permet le calcul des chemins.
Le module `Sluggable` est maintenant intégré dans `Permalinkable`.
Le module `WithGitFiles` permet l'export vers Git.

{{< callout type="warning" >}}
Une localisation n'a aucune référence directe. Toutes les références (par les blocs par exemple) sont liées à l'objet (`Person` ici).
{{</ callout >}}

Les localisations n'incluent PAS le module `AsIndirectObject`, même si des comportements sont communs. 
Ils n'ont pas de connexions, pas de références, ne sont pas sauvegardés directement (on les sauve via les formulaires des objets eux-même)
Et c'est assez compliqué comme ça conceptuellement, on n'en rajoute pas, merci.

#### Objet direct

L'exemple suivant est le `Post`, mais cela fonctionne à l'identique pour les autres objets directs (`Event`, `Project`...).

La question est de savoir si le module `AsDirectObject` continue d'inclure en cascade les modules `WithGit` et `WithGitFiles`. 
Il parait évident qu'il n'y a plus de `WithGitFiles` car ce sont les localisations qui vont prendre la main. 
Par contre c'est toujours l'objet lui-même qui est sauvé, et qui déclenche l'envoi sur Git.
Il faut donc renlever le `WithGitFiles` mais laisser le `WithGit`.

``` ruby {filename="app/models/communication/website/post.rb"}
class Communication::Website::Post < ApplicationRecord
  include AsDirectObject
  include Localizable
```

Le module `AsDirectObject` gère les dépendances, les références et les connexions, et les fonctionnalités de synchronisation.

Le module `Localizable` gère le lien aux localisations (dans ce cas, c'est le modèle `University::Person::Localization`).

{{< callout type="warning" >}}
Il n'y a plus les modules...
- `WithGitFiles` parce que l'objet lui-même n'est pas envoyé sur Git, ce sont ses localisations qui sont envoyées.  
- `Permalinkable` parce que le path et le slug dépendent de la langue.
- `Contentful` parce que l'objet lui-même n'a plus de blocs, ce sont ses localisations qui en ont.
- `Backlinkable` parce que ce sont les localisations, plus précisément les blocks liés aux localisations, qui créent les backlinks.
{{</ callout >}}

``` ruby {filename="app/models/communication/website/post/localization.rb"}
class Communication::Website::Post::Localization < ApplicationRecord
  include AsLocalization
  include Contentful
  include Permalinkable
  include WithGitFiles
```
Identique à `University::Person::Localization`.
La plupart des localisations partagent ces modules, mais pour autant ce ne sont pas des caractéristiques intrinsèques. 
C'est pourquoi on ne les ajoute pas au module `AsLocalization`.

#### Backlinks

Les backlinks lient maintenant une localisation (de `Post` par exemple) à une localisation de `Person`. 
On fait cette jointure sur les localisations parce qu'un bloc d'une actualité en italien peut pointer vers une personne sans que son équivalent français pointe vers la même personne. 
Du coup les backlinks sont dépendants du contexte linguistique.

### Controllers

Les contrôleurs utilisent le concern `Admin::Localizable`

```ruby {filename="app/controllers/admin/university/organizations_controller.rb"}
class Admin::University::OrganizationsController < Admin::University::ApplicationController
  include Admin::Localizable
```

Il n'y a plus de filtre sur la langue à faire, puisque les objets eux-mêmes ne sont pas dépendants de la langue.
Le scope suivant disparaît :
```
.for_language_id(current_university.default_language_id)
```

Cela a un impact sur la recherche, avec le scope suivant (TODO Documenter ce scope quand il est bien calé)
```
.in_closest_language_id(current_language.id)
```

Les notices utilisent la méthode suivante pour afficher le nom/titre localisé : 
```
.to_s_in(current_language)
```

Les params distinguent propriétés localisées et non localisées : 

```ruby {filename="app/controllers/admin/university/organizations_controller.rb"}
  def organization_params
    params.require(:university_organization)
          .permit(
            :active, :siren, :kind, :address, :zipcode, :city, :country, :phone, :email, category_ids: [],
            localizations_attributes: [
              :id, # Cet identifiant sert à définir quelle localisation est éditée (donc quelle langue) 
              :name, :long_name, :slug, :meta_description, :summary, :text,
              :address_name, :address_additional,
              :url, :linkedin, :twitter, :mastodon,
              :logo, :logo_delete, :logo_infos,
              :logo_on_dark_background, :logo_on_dark_background_delete, :logo_on_dark_background_infos,
              :shared_image, :shared_image_delete,
              :language_id
            ]
          )
  end
```

TODO suppression des objets et/ou des localisations.
Faut-il supprimer l'objet et toutes ses locas, ou juste la loca en cours ?

### Views

Dans les listes (index), on doit récupérer la bonne localisation.
Ce code doit être factorisé.

``` erb {filename="app/views/admin/university/organizations/_list.html.erb"}
  <% organizations.each do |organization| 
    l10n = organization.localization_for(current_language)
    if l10n.present?
      # On a la loca dans la bonne langue
      published = true
      name = l10n.to_s
      classes = ''
      alert = ''
    else
      # On n'a pas la loca, on se rabat sur l'original
      published = false
      l10n = organization.original_localization
      name = l10n.to_s
      classes = 'fst-italic'
      alert = t('localization.creation_alert')
    end
    %>
```

Dans les pages d'affichage (show), il faut choisir ce qui vient de l'objet et ce qui vient de la localisation.

``` erb {filename="app/views/admin/university/organizations/show.html.erb"}
<% content_for :title, @l10n %>
<%= render 'admin/application/summary/show', about: @l10n %>
<%= osuny_property_show_text @l10n, :address_name, hide_blank: true %>
<%= osuny_property_show_text @organization, :address, hide_blank: true %>
<%= osuny_property_show_text @l10n, :address_additional, hide_blank: true %>
<%= osuny_property_show_text @organization, :zipcode, hide_blank: true %>
<%= osuny_property_show_text @organization, :city, hide_blank: true %>
<% if @l10n.logo.attached? %>
  <div>
    <%= osuny_label University::Organization::Localization.human_attribute_name('logo') %><br>
    <%= kamifusen_tag @l10n.logo, class: 'img-fluid img-fill bg-light img-thumbnail p-5 mb-3' %>
  </div>
<% end %>
```

Dans les formulaires, on doit traiter 2 objets en même temps, l'`about` et sa localisation.

``` erb {filename="app/views/admin/university/organizations/new.html.erb"}
<% content_for :title, University::Organization.model_name.human %>
<%= render 'form', 
            organization: @organization, 
            l10n: @l10n %>
```

``` erb {filename="app/views/admin/university/organizations/edit.html.erb"}
<% content_for :title, @l10n %>
<%= render 'form', 
            organization: @organization, 
            l10n: @l10n %>
```

Le formulaire utilise un système d'imbrication avec `simple_fields_for`, 
qui fournit `f` comme formulaire de l'objet et `lf` comme formulaire de la localisation.

``` erb {filename="app/views/admin/university/organizations/_form.html.erb"}
<%= simple_form_for [:admin, organization] do |f| %>
  <%= f.simple_fields_for :localizations, l10n do |lf| %>
    <%= lf.hidden_field :language_id, value: current_language.id %>
    <%= lf.input :name %>
    <%= render 'admin/application/summary/form', f: lf, about: l10n %>
    <%= lf.input :address_name %>
    <%= f.input :address %>
    <%= lf.input :address_additional %>
    <%= f.input :zipcode %>
    <%= f.input :city %>
    <%= lf.input :logo,
                as: :single_deletable_file,
                input_html: { accept: default_images_formats_accepted },
                preview: 200,
                resize: false,
                direct_upload: true %>
    <%= submit f %>
  <% end %>
<% end %>
```

Le static fait le même travail de picking entre objet et localisation.
Attention, `@about` représente la localisation et non pas l'organisation.

``` erb {filename="app/views/admin/university/organization/localizations/static.html.erb"}
<%
version = 2
organization = @about.about
cache [@about, organization, @website.id, version] do
%>---
<%= render 'admin/application/static/title' %>
<%= render 'admin/application/static/contact_detail', variable: :address_name, data: @about.address_name, kind: ContactDetails::Base %>
<%= render 'admin/application/static/contact_detail', variable: :address, data: organization.address, kind: ContactDetails::Base %>
<%= render 'admin/application/static/contact_detail', variable: :address_additional, data: @about.address_additional, kind: ContactDetails::Base %>
<%= render 'admin/application/static/contact_detail', variable: :zipcode, data: organization.zipcode, kind: ContactDetails::Base %>
<%= render 'admin/application/static/contact_detail', variable: :city, data: organization.city, kind: ContactDetails::Base %>
<% if @about.logo.attached? %>
logo: "<%= @about.logo.blob.id %>"
<% end %>
<%= render 'admin/communication/blocks/content/static', about: @about %>
---
```

Les blocs sont attachés à la localisation, donc à l'`@about`.


## Migration

Toutes les informations localisées sont supprimées de l'organisation elle-même, et déplacées dans la localisation.
Pour gérer la migration, cela se fait en 2 passes, d'abord le déplacement, puis la suppression.

``` ruby {filename="migration"}
  # Etape 1 : créer la localisation
  create_table :university_organization_localizations, id: :uuid do |t|
    t.string  :address_additional
    t.string  :address_name
    t.string  :linkedin
    t.string  :long_name
    t.string  :mastodon
    t.text    :meta_description
    t.string  :name
    t.string  :slug
    t.text    :summary
    t.text    :text
    t.string  :twitter
    t.string  :url

    t.references :about, foreign_key: { to_table: :university_organization }, type: :uuid
    t.references :language, foreign_key: true, type: :uuid
    t.references :university, foreign_key: true, type: :uuid

    t.timestamps
  end

  # Etape 2 migrer les données
  University::Organization.find_each do |orga|
    # If "old way" translation, we set the about to the original, else if "old way" master, we take its ID.
    about_id = orga.original_id || orga.id

    l10n = University::Organization::Localization.create(
      address_additional: orga.address_additional,
      address_name: orga.address_name,
      linkedin: orga.linkedin,
      long_name: orga.long_name,
      mastodon: orga.mastodon,
      meta_description: orga.meta_description,
      name: orga.name,
      summary: orga.summary,
      slug: orga.slug,
      text: orga.text,
      twitter: orga.twitter,
      url: orga.url,
      about_id: about_id,
      language_id: orga.language_id,
      university_id: orga.university_id
    )

    # Copy from orga (old) to localization (new)
    orga.translate_contents!(l10n)
    orga.translate_other_attachments(l10n)

    # Get permalinks (for aliases)
    orga.permalinks.each do |permalink|
      new_permalink = permalink.dup
      new_permalink.target = l10n
      new_permalink.save
    end

    l10n.save
  end
```

### Objets indirects

#### Organization

Propriétés localisées 

| Propriété | Explication |
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

Propriétés non localisées 

| Propriété | Explication |
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

#### Organization::Category

Propriétés localisées 

| Propriété | Explication |
|-|-|
| name | Le nom est traduit |
| slug | Le slug dépend du name |

Propriétés non localisées 

| Propriété | Explication |
|-|-|
| parent_id | L'arbre de catégories est indépendant des localisations |
| position | La position est lié à l'arbre de catégories |

#### Person

Propriétés localisées 

| Propriété | Explication |
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

Propriétés non localisées 

| Propriété | Explication |
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

Les personnes ont des facettes : `Administrator`, `Author`, `Researcher`, `Teacher`.
Les facettes sont activées par des booléens, comme `is_author` (attention il y a des subtilités).
Ces 4 facettes permettent de gérer les 5 pages différentes possibles pour une même personne dans Hugo.
Les facettes sont donc devenues des classes qui héritent de la localisation et plus de la personne elle-même.
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

#### Person::Category

Propriétés localisées 

| Propriété | Explication |
|-|-|
| name | Le nom est traduit |
| slug | Le slug dépend du name |

Propriétés non localisées 

| Propriété | Explication |
|-|-|
| parent_id | L'arbre de catégories est indépendant des localisations |
| position | La position est lié à l'arbre de catégories |


### Objets directs

#### Post

Propriétés localisées 

| Propriété | Explication |
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

Propriétés non localisées 

| Propriété | Explication |
|-|-|
| migration_identifier | Un identifiant dans le `Post`, un autre dans la `Localization` |

#### Post::Category

Propriétés localisées 

| Propriété | Explication |
|-|-|
| featured_image | L'image d'illustration peut être une image avec des textes, comme une affiche par exemple |
| featured_image_alt | Il faut donc traduire les textes en question |
| featured_image_credit | Le crédit peut être traduit ("Photo par...", "Photo by...")
| meta_description | La description de la catégorie est dans la langue |
| name | Le nom est traduit |
| path | Le chemin est lié au slug |
| slug | Le slug dépend du name |
| summary | Le résumé est traduit |

Propriétés non localisées 

| Propriété | Explication |
|-|-|
| is_programs_root | Lié à l'arbre de catégories des formations |
| parent_id | L'arbre de catégories est indépendant des localisations |
| position | La position est lié à l'arbre de catégories |
| program_id | Lié à l'arbre de catégories des formations |

#### Page

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

Propriétés localisées 

| Propriété | Explication |
|-|-|
| breadcrumb_title | Titre traduit |
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
| slug | Le slug dépend du name |
| summary | Texte traduit |
| text | Texte traduit |
| title | Texte traduit |

Propriétés non localisées 

| Propriété | Explication |
|-|-|
| bodyclass | Gestion centralisée |
| full_width | Gestion centralisée |
| position | Gestion centralisée |
| migration_identifier | Identifiant utilisé par l'API, présent des 2 côtés |
| type | Propriété utilisée pour la STI |
