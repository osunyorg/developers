---
title: Filtres
weight: 20
---

La mécanique de filtres mise en place pour le back-office repose sur plusieurs fichiers.  
Ce document explique comment la mécanique globale est mise en place.  
Pour voir comment réaliser une implémentation dans l'admin se référer à la page [de tutoriel sur l'implémentation d'un index filtrable](/docs/admin/tutoriels/creer-un-index-filtre/).

## Un concern de model

[/app/models/concerns/filterable.erb](https://github.com/osunyorg/admin/blob/main/app/models/concerns/filterable.rb)

Ce concern doit être inclu dans chaque model sur lequel on va effectuer des filtrages ensuite. 
Il permet de définir une méthode filter_by à laquelle on passe une liste de filtres et qui va convertir ça en un chainage de scopes.  
Pour des questions de sécurité il s'assure aussi qu'on ne passe QUE des paramètres correspondants à des scopes définis dans le modèle.

## Un partial de view 

[/app/wiews/admin/application/components/filters.html.erb](https://github.com/osunyorg/admin/lob/main/app/wiews/admin/application/components/filters.html.erb)

C'est le template qui défini qu'on a un bouton affiché quelque part sur l'écran et ensuite un panel de filtres affichés en offcanvas. 
Ce layout prend un yield et est donc utilisable après dans les vues avec un appel du type :

```erb {filename="app/views/admin/education/teachers/_filters.html.erb"}
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

## Un helper de vue

[/app/helpers/admin/filters_helper.rb](https://github.com/osunyorg/admin/lob/main/app/helpers/admin/filters_helper.rb)

Il définit un helper `filters_panel` qui va injecter le template dont on parle juste au dessus.  
Il définit aussi un helper `active_filters_count` qui permet de compter le nombre de filtres actifs actuellement (utilisé dans le bouton de filtres du template par exemple).  
Enfin il définit un helper `render_filter` qui fait passe plat avec tous les filtres typés, donc :
```erb
<%= render_filter f,
                 :string,
                 :for_search_term,
                 label: t('search')
                 %>
```
est équivalent à 
```erb
<%= render_string_filter f,
                         :for_search_term,
                         label: t('search')
                         %>
```


