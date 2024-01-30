---
title: Créer un objet indirect
---

Cet exemple s'appuie sur la gestion de campus, avec des objets `administration/Location`.

## Créer le modèle et la migration
```Bash { filename="terminal" }
rails g model administration/Location 
  name:string 
  slug:string 
  summary:text
  university:references 
  address:string 
  city:string 
  zipcode:string
  country:string 
  latitude:float 
  longitude:float 
  phone:string 
  url:string 
```
La propriété `university` permet de gérer le multitenant.
Les propriétés `name` et `slug` permettent de créer les permaliens.

## Nourrir le modèle

```Ruby { filename="app/models/administration/location.rb" }
class Administration::Location < ApplicationRecord
  include AsIndirectObject
  include Sanitizable
  include Contentful
  include Sluggable
  include WebsitesLinkable
  include WithBlobs
  include WithCountry
  include WithGitFiles
  include WithGeolocation
  include WithPermalink
  include WithUniversity

  scope :ordered, -> { order(:name) }

  validates :name, :address, :city, :zipcode, :country, presence: true

  def to_s
    "#{name}"
  end

  def git_path(website)
    "#{git_path_content_prefix(website)}locations/#{slug}/_index.html" if for_website?(website)
  end

  def dependencies
    active_storage_blobs +
    contents_dependencies +
    programs +
    schools
  end

  def references
    []
  end
end
```

- `AsIndirectObject` est en charge de toute la logique des objets indirects.
- `Sanitizable` sécurise les entrées dans la base de donnnées.
- `Contentful`ajoute les blocs et les titres.
- `Sluggable` génère les slugs uniques à partir du nom.
- `WebsitesLinkable` permet de créer des sites Web à propos de l'objet.
- `WithBlobs` permet de lister les blobs dans les dépendances.
- `WithCountry` gère le champ `country` avec des codes ISO officiels.
- `WithGitFiles` permet de générer des fichiers statiques et de les mettre à jour.
- `WithGeolocation` remplit `latitude` et `longitude` à partir de l'adresse.
- `WithPermalink` crée les chemins et garde trace des redirections.
- `WithUniversity` gère le multi-tenant.

La méthode `git_path(website)` est utilisée pour définir le chemin du fichier sur le référentiel git. 

Les dépendances sont les objets nécessaires pour l'affichage.

Les références sont les objets qui peuvent citer l'objet courant.

## Créer le controller

En partant d'une copie d'un autre controller.

```Ruby { filename="app/controllers/admin/administration/locations_controller.rb" }
class Admin::Administration::LocationsController < Admin::Administration::ApplicationController
  load_and_authorize_resource class: Administration::Location,
                              through: :current_university

  def index
    breadcrumb
  end

  def show
    breadcrumb
  end

  def static
    @about = @location
    @website = @location.websites&.first
    render_as_plain_text
  end

  def new
    breadcrumb
  end

  def edit
    breadcrumb
    add_breadcrumb t('edit')
  end

  def create
    @location.university = current_university
    if @location.save
      redirect_to [:admin, @location],
                  notice: t('admin.successfully_created_html', model: @location.to_s)
    else
      breadcrumb
      render :new, status: :unprocessable_entity
    end
  end

  def update
    if @location.update(location_params)
      redirect_to [:admin, @location],
                  notice: t('admin.successfully_updated_html', model: @location.to_s)
    else
      breadcrumb
      add_breadcrumb t('edit')
      render :edit, status: :unprocessable_entity
    end
  end

  def destroy
    @location.destroy
    redirect_to admin_education_locations_url,
                notice: t('admin.successfully_destroyed_html', model: @location.to_s)
  end

  private

  def breadcrumb
    super
    add_breadcrumb Administration::Location.model_name.human(count: 2), admin_administration_locations_path
    breadcrumb_for @location
  end

  def location_params
    params.require(:administration_location)
          .permit(:name, :address, :zipcode, :city, :country, :url, :phone, school_ids: [], program_ids: [])
  end
end
```

## Créer les vues

En partant d'une copie d'un autre dossier de vues.

```Ruby { filename="app/views/admin/administration/locations/_form.html.erb" }
<%= simple_form_for [:admin, location] do |f| %>
  <%= f.error_notification %>
  <%= f.error_notification message: f.object.errors[:base].to_sentence if f.object.errors[:base].present? %>
  <div class="row mb-5">
    <div class="col-lg-4">
      <%= f.input :name %>
      <%= render 'admin/application/summary/form', f: f, about: location %>
    </div>
    <div class="col-lg-4">
      <%= f.input :address %>
      <div class="row pure__row--small">
        <div class="col-md-4">
          <%= f.input :zipcode %>
        </div>
        <div class="col-md-8">
          <%= f.input :city %>
        </div>
      </div>
      <%= f.input :country, input_html: { class: 'form-select' } %>
    </div>
    <div class="col-lg-4">
      <%= f.input :phone %>
      <%= f.input :url %>
    </div>
  </div>
  <% content_for :action_bar_right do %>
    <%= submit f %>
  <% end %>
<% end %>
```

```Ruby { filename="app/views/admin/administration/locations/_list.html.erb" }
<div class="table-responsive">
  <table class="<%= table_classes %>">
    <thead>
      <tr>
        <th><%= Administration::Location.human_attribute_name('name') %></th>
        <th><%= Administration::Location.human_attribute_name('address') %></th>
        <th></th>
      </tr>
    </thead>
    <tbody>
      <% locations.ordered.each do |location| %>
        <tr>
          <td><%= link_to location, [:admin, location] %></td>
          <td><%= location.full_street_address %></td>
          <td class="text-end">
            <div class="btn-group" role="group">
              <%= edit_link location %>
              <%= destroy_link location %>
            </div>
          </td>
        </tr>
      <% end %>
    </tbody>
  </table>
</div>
```

```Ruby { filename="app/views/admin/administration/locations/edit.html.erb" }
<% content_for :title, @location %>
<%= render 'form', location: @location %>
```

```Ruby { filename="app/views/admin/administration/locations/index.html.erb" }
<% content_for :title, Administration::Location.model_name.human(count: 2) %>
<%= render 'admin/administration/locations/list', locations: @locations %>
<% content_for :action_bar_right do %>
  <%= create_link Administration::Location %>
<% end %>
```

```Ruby { filename="app/views/admin/administration/locations/new.html.erb" }
<% content_for :title, Administration::Location.model_name.human %>
<%= render 'form', location: @location %>
```

```Ruby { filename="app/views/admin/administration/locations/show.html.erb" }
<% content_for :title, @location %>
<div class="row">
  <div class="col-lg-4">
    <%= osuny_panel t('metadata') do %>
      <%= osuny_label Education::School.human_attribute_name('address') %>
      <p>
        <%= @location.address %><br>
        <%= @location.zipcode %> <%= @location.city %><br>
        <%= @location.country %>
      </p>
      <% if @location.phone.present? %>
        <%= osuny_label Education::School.human_attribute_name('phone') %>
        <p><%= @location.phone %></p>
      <% end %>
      <% if @location.url.present? %>
        <%= osuny_label Administration::Location.human_attribute_name('url') %>
        <p><%= link_to @location.url, @location.url, target: :_blank %></p>
      <% end %>
    <% end %>
  </div>
  <% if @location.websites.any? %>
    <div class="col-lg-4">
      <%= osuny_panel Administration::Location.human_attribute_name('websites') do %>
        <ul class="list-unstyled">
          <% @location.websites.each do |website| %>
            <li><%= link_to website, [:admin, website] %></li>
          <% end %>
        </ul>
      <% end %>
    </div>
  <% end %>
</div>
<%= render 'admin/communication/blocks/content/editor', about: @location %>
<%= render 'admin/application/connections/list', about: @location %>
<% content_for :action_bar_left do %>
  <%= destroy_link @location %>
  <%= static_link static_admin_administration_location_path(@location) %>
<% end %>
<% content_for :action_bar_right do %>
  <%= edit_link @location %>
<% end %>
```

```Ruby { filename="app/views/admin/administration/locations/show.html.erb" }
---
title: >
  <%= prepare_text_for_static @about.name %>
<%= render 'admin/application/static/permalink' if @website %>
<%= render 'admin/application/static/design', full_width: true, toc_offcanvas: true %>
<% if @website %>
<%= render 'admin/application/static/breadcrumbs', 
            pages: @website.special_page(Communication::Website::Page::AdministrationLocation).ancestors_and_self,
            current: @about %>
<% end %>
<%= render 'admin/communication/blocks/content/static', about: @about %>
---
```

## Déclarer la nouvelle partie

```Ruby { filename="app/models/administration.rb" }
  def self.parts
    [
      [Administration::Location, :admin_administration_locations_path],
      [Administration::Qualiopi, :admin_administration_qualiopi_criterions_path],
    ]
  end
```

Cela permet de construire le menu et les pages d'accueil de royaumes.

## Créer la page spéciale

C'est la page qui va apparaître dans l'arborescence du site Web pour lister les objets.

```Ruby { filename="app/models/communication/website/page/administration_location.rb" }
class Communication::Website::Page::AdministrationLocation < Communication::Website::Page

  def is_necessary_for_website?
    website.about && website.about&.respond_to?(:administration_locations)
  end

  def editable_width?
    false
  end

  def full_width_by_default?
    true
  end

  def full_width
    true
  end

  def dependencies
    super +
    [website.config_default_languages] +
    website.administration_locations
  end

  protected

  def current_git_path
    @current_git_path ||= "#{git_path_prefix}locations/_index.html"
  end

end
```

Il faut aussi la déclarer dans le fichier qui les liste (le fichier est écourté pour la lisibilité).

```Ruby { filename="app/models/communication/website/page/with_type.rb" }
module Communication::Website::Page::WithType
  extend ActiveSupport::Concern
  included do
    TYPES = [
      ...
      # Administration
      Communication::Website::Page::AdministrationLocation,
      ...
    ]
  end
end
```

## Définir la structure des permaliens

```Ruby { filename="app/models/communication/website/permalink/location.rb" }
class Communication::Website::Permalink::Location < Communication::Website::Permalink
  def self.required_in_config?(website)
    website.has_administration_locations?
  end

  def self.static_config_key
    :locations
  end

  # /campus/:slug/
  def self.pattern_in_website(website, language)
    "/#{website.special_page(Communication::Website::Page::AdministrationLocation, language: language).slug_with_ancestors}/:slug/"
  end
end
```

```Ruby { filename="app/models/communication/website/permalink.rb" }
class Communication::Website::Permalink < ApplicationRecord
  MAPPING = {
    ...
    "Administration::Location" => Communication::Website::Permalink::Location,
    ...
  }
end
```

## Associer l'objet au site Web

```Ruby { filename="app/models/communication/website/with_associated_objects.rb" }
  ...
  def administration_locations
    has_administration_locations? ? about.administration_locations : Administration::Location.none
  end
  ...
  def has_administration_locations?
    about && about.has_administration_locations?
  end
  ...
```

Il faut aussi le déclarer dans les méthodes à implémenter pours les objets qui peuvent être liés à un site Web.

```Ruby { filename="app/models/concerns/websites_linkable.rb" }
  ...
  def has_administration_locations?
    raise NotImplementedError
  end
  ...
```

Tous les objets qui intègrent le concern `WebsitesLinkable` doivent intégrer la méthode, même si c'est pour renvoyer false.

## Ajouter les locales

```YAML { filename="config/locales/administration/fr.yml" }
  activerecord:
    models:
      administration/location:
        one: Site
        other: Campus
    ...
    attributes:
      administration/location:
        address: Adresse
        city: Ville
        country: Pays
        name: Nom
        phone: Téléphone
        programs: Formations dispensées
        schools: Écoles
        url: Site Web
        websites: Sites Web associés
        zipcode: Code postal
```