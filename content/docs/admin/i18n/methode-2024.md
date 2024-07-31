---
title: Méthode 2024
weight: 1
---
{{< callout type="info" >}}
Méthode appliquée à partir de l'été 2024.
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
