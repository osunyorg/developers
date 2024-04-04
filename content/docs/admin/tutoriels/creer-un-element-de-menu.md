---
title: Créer un élément de menu
---

## Ajouter le kind au modèle 

Il faut ajouter un `kind` à la liste.
```Ruby { filename="app/models/communication/website/menu/item.rb" }
  enum kind: {
    blank: 0,
    url: 10,
    page: 20,
    program: 31,
    diploma: 33,
    category: 41,
    post: 42,
    volume: 61,
    paper: 63,
    location: 70
  }, _prefix: :kind
```
Il faut aussi ajouter une icône correspondant à ce nouveau kind (méthode `self.icon_for(kind)`).

```Ruby { filename="app/models/communication/website/menu/item.rb" }
  ICONS = {
    'blank' => Icon::COMMUNICATION_WEBSITE_MENU_BLANK,
    'category' => Icon::COMMUNICATION_WEBSITE_POST,
    'diploma' => Icon::EDUCATION_DIPLOMA,
    'location' => Icon::ADMINISTRATION_CAMPUS,
    'page' => Icon::COMMUNICATION_WEBSITE_PAGE,
    'paper' => Icon::RESEARCH_LABORATORY,
    'post' => Icon::COMMUNICATION_WEBSITE_POST,
    'program' => Icon::EDUCATION_PROGRAM,
    'url' => Icon::COMMUNICATION_WEBSITE_MENU_URL,
    'volume' => Icon::RESEARCH_LABORATORY
  }
```

## Ajouter les traductions

Ne pas oublier d'ajouter le nouveau cas à l'enum communication:website:menu:item:kind dans chacun des fichiers de locale.

```Ruby { filename="config/locales/communication/fr.yml" }
fr:
  enums:
    communication:
      website:
        menu:
          item:
            kind:
              location: Site (campus)
```

```Ruby { filename="config/locales/communication/en.yml" }
en:
  enums:
    communication:
      website:
        menu:
          item:
            kind:
              location: Location (campus)
```

## Pour les objets indirects

Il faut indiquer au website la condition correspondant à l'affichage ou non de l'option.  
Ca se passe dans le trait `with_menu` du website (app/models/communication/website/with_menus.rb).  
Il faut rajouter une condition d'affichage sous la forme `menu_item_kind_NOUVEAUKIND?`.  

Par exemple :

```Ruby { filename="app/models/communication/website/with_menus.rb" }
def menu_item_kind_location?
  has_administration_locations?
end
```

## Pour les objets à choisir (direct ou indirects) 

Éléments nécessitant un target plus précis (1 page, 1 événement, 1 site de campus...)
1. Si on doit par la suite sélectionner un sous-élément, il faut ajouter le nouveau kind à la méthode `has_about?` du modèle Communication::Website::Menu::Item
2. Ensuite fans le formulaire d'édition de l'item (app/views/admin/communication/websites/menus/items/_form.html.erb) il faut ajouter la collection des objets possibles
3. Enfin dans le javascript qui gère le changement de destination de l'élement de menu il faut ajouter également le cas (app/views/admin/communication/websites/menus/items/kind_switch.js.erb)

## À propos des dépendances et références
Dans les menu item on n'a pas besoin de se soucier des dépendances ni des références.  
Comme on ne peut ajouter comme élément de menu que des éléments déjà connectés à un website on devrait retrouver le target dans le repo Git.  
Ensuite si on déconnecte l'élément (par exemple un diplôme ou un campus) du website, l'élément sera nettoyé par d'autres biais. L'entrée du menu demeurera (jusqu'à ce qu'elle soit supprimée/remplacée manuellement), mais l'entrée sera ignorée dans le thème et donc pas affichée.  

## Exemple complet
Pour un exemple complet d'ajout d'un élément indirect au menu, se référer à cette [PR](https://github.com/osunyorg/admin/pull/1754)