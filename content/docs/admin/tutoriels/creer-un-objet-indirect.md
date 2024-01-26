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

