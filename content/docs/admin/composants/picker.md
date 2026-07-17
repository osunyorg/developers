---
title: Sélecteur (picker)
---

## Introduction

Le sélecteur est une mécanique standardisée de sélection d'objet.

Il est composé de 3 composants :
- les paramètres, qui permettent de chercher, filtrer et trier
- la pagination, qui permet de gérer les gros volumes
- les résultats, qui affichent les objets

Le système s'articule avec les [filtres](/docs/admin/composants/filters/) afin d'utiliser les mêmes requêtes SQL et les mêmes syntaxes de paramètres.

## Interface

Le sélecteur ouvre une modale en pleine page.

L'écran est divisé en 2 : 
- paramètres (à gauche en desktop)
- résultats (à droite en desktop)

La pagination est intégrée aux résultats.

## Implémentation

### JSON pivot

Ce fichier est le cœur du contrat, il nourrit l'application Vue

```json{filename="URL /admin/fr/communication/files/picker.json"}
{
  "parameters": {
    "search": {
      "term": "",
      "query_parameters": "&filters[for_search_term]="
    },
    "filters": [
      {
        "id": "categories",
        "name": "Catégories",
        "values": [
          {
            "id": "cat-1",
            "name": "Catégorie 1",
            "selected": false
          }
        ]
      }
    ],
    "sorts": [
      {
        "id": "date-desc",
        "name": "Les plus récents d'abord",
        "selected": true
      },
      {
        "id": "date-asc",
        "name": "Les plus anciens d'abord",
        "selected": false
      }
    ],
    "query_parameters": ""
  },
  "pagination": {
    "current_page": 1,
    "limit_value": 2,
    "total_count": 4,
    "total_pages": 2,
    "query_parameters": "&page=1"
  },
  "results": [
    {
      "data": {
        "communication_file_id": "6bdb40de-0736-46b4-8d73-18814f72633e",
        "filename": "Bohemes_Brodees.pdf",
        "name": "Bohemes_Brodees.pdf"
      },
      "snippet": "<div class=\"card card--osuny card--horizontal\">\n  <div class=\"osuny__thumbnail\n              osuny__thumbnail--small\n              osuny__thumbnail--cropped\">\n    <span class=\"osuny__thumbnail__icon\">\n      <i class=\"bi bi-file-earmark-pdf\" title=\"application/pdf\"></i>\n    </span>\n  </div>\n  <div class=\"card-body\">\n    <a class=\"stretched-link\" data-confirm=\"\" href=\"/admin/fr/communication/files/6bdb40de-0736-46b4-8d73-18814f72633e\">Bohemes_Brodees.pdf</a>\n  </div>\n</div>\n"
    },
    {
      "data": {
        "communication_file_id": "e2b7940d-9188-4e6a-9557-d78a59acff77",
        "filename": "c08007262.pdf",
        "name": "c08007262.pdf"
      },
      "snippet": "<div class=\"card card--osuny card--horizontal\">\n  <div class=\"osuny__thumbnail\n              osuny__thumbnail--small\n              osuny__thumbnail--cropped\">\n    <span class=\"osuny__thumbnail__icon\">\n      <i class=\"bi bi-file-earmark-pdf\" title=\"application/pdf\"></i>\n    </span>\n  </div>\n  <div class=\"card-body\">\n    <a class=\"stretched-link\" data-confirm=\"\" href=\"/admin/fr/communication/files/e2b7940d-9188-4e6a-9557-d78a59acff77\">c08007262.pdf</a>\n  </div>\n</div>\n"
    }
  ]
}
```
Il manquera certainement les infos de layout (row, col) pour l'affichage des personnes en grille, par exemple.

Les paramètres listent les possibilités d'action, en fournissant les paramètres de requête prêts à l'usage.
L'idée est que l'app Vue traite des objets abstraits, sans logique métier.

La pagination fournit toutes les infos nécessaires aux gros volumes.

Les résultats fournissent le snippet à afficher et les data à renvoyer lors de la sélection.

### Vues Rails

```html{filename="app/views/admin/communication/blocks/components/file/_edit.html.erb"}
<Picker
  v-model="<%= model %>.<%= property %>"
  kind="files"
  endpoint="<%= picker_admin_communication_files_path(format: :json) %>">
</Picker>
```

On utilise le picker dans une app Vue, en lui fournissant 3 paramètres :
- son modèle, qui indique la donnée à mettre à jour
- son kind, qui indique les locas à utiliser
- son endpoint, qui indique la source du JSON

### Application Vue.js

L'application est dans `app/javascript/apps/picker/Picker.vue`.
Elle utilise les 3 composants, `Parameters`, `Pagination` et `Results`.

### Routes et controllers

Les fichiers pivots utilisent des URLs de type : 
/admin/fr/communication/files/picker.json

Une app de test (ça évite les multiples clics de l'éditeur) est disponible sur :
/admin/fr/communication/files/picker

Le controller déclare l'action comme ça :
```rb{filename="app/controllers/admin/communication/library/files_controller.rb"}
def picker
  @picker = Communication::File::Picker.new(
    objects: @files,
    language: current_language,
    params: params
  )
end
```

Ça permet de fonctionner un peu comme l'index, mais avec des spécificités.

Dans la vue, on fait ça :
```rb{filename="app/views/admin/communication/library/files/picker.json.jbuilder"}
json.parameters @picker.parameters
json.pagination @picker.pagination
json.results do
  json.list @picker.results do |file|
    l10n = file.localized_in(current_language)
    json.data do
      json.communication_file_id file.id
      json.filename l10n.original_filename
      json.name l10n.name
    end
    json.snippet render(
        partial: 'admin/communication/library/files/file',
        locals: { file: file },
        formats: [:html]
      )
  end
end
```

Ça permet de gérer les data et le snippet dans une vue, c'est à sa place.

```html{filename="app/views/admin/communication/library/files/picker.html.erb"}
<% content_for :title, 'Test du picker' %>
<div
  id="picker-test-app"
  data-kind="files"
  data-endpoint="<%= picker_admin_communication_files_path(format: :json) %>">
</div>
```

### Modèle

On s'appuie sur un modèle, mais ça pourrait aussi être un service. 
L'avantage du modèle est d'être nativement dans le bon namespace, ici `Communication::File::Picker`.

```ruby{filename="app/models/communication/file/picker.rb"}
class Communication::File::Picker
  attr_reader :objects, :language, :params

  def initialize(objects:, language:, params:)
    ...
  end

  def parameters
    ...
  end

  def pagination
    ...
  end

  def results
    @results ||= objects.filter_by(params[:filters], language)
                        .ordered(language)
                        .page(params[:page])
                        .per(2)
  end
```

Il y a 3 méthodes publiques, permettant d'accéder aux données.
- parameters, qui fournit toutes les options d'entrée
- pagination, qui indique le nombre de résultats, la page courante, etc.
- results, qui applique les paramètres et la pagination
La méthode results utilise `filter_by`.

Il reste à définir les paramètres et à tout brancher correctement.
