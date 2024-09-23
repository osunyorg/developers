---
title: Créer un index filtré
---

Ce document présente comment réaliser une implémentation d'index filtré dans l'admin d'Osuny.  
Pour avoir une vue plus globale et théorique de la mécanique, se référer à la page [d'architecture](/docs/admin/architecture/filters/).


## Controllers :
Dans l'action d'index ajouter un scope `.filter_by(params[:filters], current_language)`.

## Views :
- Dans l'`index.html.erb` ajouter une ligne 
`<%= render 'filters', current_path: admin_ROUTE_VERS_L_INDEX_path %>`
- Créer à côté un partial `_filters.html.erb`
Le fichier de filtre doit avoir cette structure :
```
<%= simple_form_for :filters, url: current_path, method: :get do |f| %>
  <%= filters_panel current_path: current_path, active_filters_count: active_filters_count do |form| %>

    <%= render_filter f,
                      :string,
                      :for_search_term,
                      label: t('search')
                      %>

    # ET LES DIFFERENTS FILTRES

    <% end %>
<% end %>
```

## Models :
- Include le concern Filterable : `include Filterable`
- Créer les scopes dont on a besoin, en rapport avec le contenu mis dans la vue de _filters.html.erb. Tous ces copes créés doivent avoir un 2ème paramètre language optionnel. J'ai pris l'habitude d'avoir des scopes dédiés aux filtres qui commencent pas `for_xxx`.
Ca donne des scope comme ceci par exemple :
```
scope :for_search_term, -> (term, language) {
  joins(:university)
    .joins(:localizations)
    .where(communication_website_localizations: { language_id: language.id })
    .where("
      unaccent(universities.name) % unaccent(:term) OR
      unaccent(communication_website_localizations.name) % unaccent(:term) OR
      unaccent(communication_websites.url) % unaccent(:term)
    ", term: "%#{sanitize_sql_like(term)}%")
}
```

ou

```
scope :for_about_type, -> (about_type, language = nil) {
  where(about_type: about_type)
}
```