---
title: Créer une fonctionnalité de site Web
---

Cet exemple s'appuie sur les portfolios.

## Ajouter la fonctionnalité au site

```Bash { filename="terminal" }
rails g migration add_feature_portfolio_to_communication_websites feature_portfolio:boolean 
```

```Ruby { filename="db/migrate/20240307053354_add_feature_portfolio_to_communication_websites.rb" }
class AddFeaturePortfolioToCommunicationWebsites < ActiveRecord::Migration[7.1]
  def change
    add_column :communication_websites, :feature_portfolio, :boolean, default: false
  end
end
```

Ajout au controller

Ajout au formulaire

Ajout aux locales

## Créer les modèles

Les sauts de lignes sont ajoutés pour la lisibilité de la commande.

```Bash { filename="terminal" }
rails g model communication/website/portfolio/Project 
                title:string 
                slug:string 
                featured_image_alt:text 
                featured_image_credit:text 
                year:integer 
                meta_description:text 
                published:boolean 
                summary:text 
                communication_website:references 
                language:references 
                original:references 
                university:references
```

```Bash { filename="terminal" }
rails g model communication/website/portfolio/Category 
                name:string 
                slug:string 
                featured_image_alt:text 
                featured_image_credit:text 
                meta_description:text 
                is_programs_root:boolean 
                path:string 
                position:integer summary:text 
                communication_website:references 
                language:references  original:references
                parent:references 
                university:references
```

## Router

```Ruby { filename="config/routes/admin/communication.rb" }
    namespace :portfolio, path: '/:lang/portfolio' do
      resources :projects, controller: '/admin/communication/websites/portfolio/projects' do
        member do
          get :static
          post :duplicate
          post :publish
        end
      end
      resources :categories, controller: '/admin/communication/websites/portfolio/categories' do
        collection do
          post :reorder
        end
        member do
          get :children
          get :static
        end
      end
    end
```

## Localiser la fonctionnalité

Ajout de la gestion de locales dans le namespace, nécessaire pour les traductions de l'interface

```Ruby { filename="app/models/communication/website/portfolio.rb" }
module Communication::Website::Portfolio
  extend ActiveModel::Naming
  extend ActiveModel::Translation

  def self.table_name_prefix
    "communication_website_portfolio_"
  end
end
```

Ajout des termes dans les traductions françaises et anglaises

```YML { filename="config/locales/communication/fr.yml" }
fr:
  activemodel:
    models:
      communication/website/portfolio: 
        one: Portfolio
        other: Portfolio
```

Idem en anglais

## Ajouter l'icône

Le fichier est incomplet, les points de suspension indiquent du code ellipsé.

```Ruby { filename="app/services/icon.rb" }
class Icon
  ...
  COMMUNICATION_WEBSITE_PORTFOLIO = 'fas fa-briefcase'
  ...
end
```

## Créer les menus


```Ruby { filename="app/views/admin/communication/websites/_sidebar.html.erb" }
...
navigation << {
  title: Communication::Website::Portfolio.model_name.human(count: 2),
  path: admin_communication_website_portfolio_projects_path(website_id: @website),
  icon: Icon::COMMUNICATION_WEBSITE_PORTFOLIO,
  ability: can?(:read, Communication::Website::Portfolio::Project)
} if @website.feature_portfolio
...
```
## Créer les controlleurs 