---
title: API Osuny
---

## Contexte

L'API d'Osuny permet d'interagir avec l'instance de façon programmatique.
Pour assurer la sécurité des échanges, on crée une app sur l'instance concernée, avec le chemin `/admin/university/apps`.
Cette app génère un jeton secret, qu'il faut fournir pour utiliser les méthodes authentifiées de l'API.

https://demo.osuny.org/api/osuny/v1/

## Documentation de l'API Osuny

La documentation de l'API est :
1. produite avec [RSpec](https://rspec.info)
2. transformée par la gem [rswag](https://github.com/rswag/rswag)
3. publiée au format [OpenAPI](https://www.openapis.org) 
4. consultable avec [swagger-ui](https://github.com/swagger-api/swagger-ui) (produit par [Swagger](https://swagger.io))

Le workflow part des spécifiations dans le dossier `spec`.

{{< filetree/container >}}
  {{< filetree/folder name="spec" >}}
    {{< filetree/folder name="requests" state="open" >}}
      {{< filetree/folder name="communication" state="open" >}}
        {{< filetree/folder name="websites" state="open" >}}
          {{< filetree/file name="posts_spec.rb" >}}
        {{< /filetree/folder >}}
        {{< filetree/file name="websites_spec.rb" >}}
      {{< /filetree/folder >}}
      {{< filetree/file name="osuny_spec.rb" >}}
    {{< /filetree/folder >}}
    {{< filetree/file name="rails_helper.rb" >}}
    {{< filetree/file name="spec_helper.rb" >}}
    {{< filetree/file name="swagger_helper.rb" >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

```ruby {filename="spec/requests/osuny/v1/communication/websites/posts_spec.rb"}
require 'swagger_helper'

RSpec.describe 'Communication::Website::Post' do
  fixtures :all

  path '/communication/websites/{website_id}/posts' do
    get "Lists a website's posts" do
      tags 'Communication::Website::Post'
      security [{ api_key: [] }]
      let("X-Osuny-Token") { university_apps(:default_app).token }

      parameter name: :website_id, in: :path, type: :string, description: 'Website identifier'
      let(:website_id) { communication_websites(:website_with_github).id }

      response '200', 'Successful operation' do
        run_test!
      end

      response '401', 'Unauthorized. Please make sure you provide a valid API key.' do
        let("X-Osuny-Token") { 'fake-token' }
        run_test!
      end

      response '404', 'Website not found' do
        let(:website_id) { 'fake-id' }
        run_test!
      end
    end
  end
end
```

Les tests sont utilisés pour générer des fichiers `openapi.json`, par exemple sur https://demo.osuny.org/api/docs/osuny/v1/openapi.json, avec la commande suivante.

``` {filename="bash"}
RSWAG_DRY_RUN=0 rake rswag:specs:swaggerize
```

Pour générer la spécification OpenAPI en évitant les tests, il suffit d'omettre la variable d'environnement `RSWAG_DRY_RUN`. Cela permet d'avoir un aperçu rapide dans Swagger sans les exemples de réponse.

``` {filename="bash"}
rake rswag:specs:swaggerize
```

## Utilisation de l'API Osuny
