---
title: Créer un élément de menu
---

## Ajouter le kind au modèle (app/models/communication/website/menu/item.rb)
Il faut ajouter un `kind` à la liste.
Il faut aussi ajouter une icône correspondant à ce nouveau kind (méthode `self.icon_for(kind)`).

## Ajouter le kind aux fichiers de locales
Ne pas oublié d'ajouter le nouveau cas à l'enum communication:website:menu:item:kind dans chacun des fichiers de locale.

## Cas des objets indirects
Il faut indiquer au website la condition correspondant à l'affichage ou non de l'option.  
Ca se passe dans le trait `with_menu` du website (app/models/communication/website/with_menus.rb).  
Il faut rajouter une condition d'affichage sous la forme `menu_item_kind_NOUVEAUKIND?`.  
Par exemple :
```Ruby { filename="app/models/communication/website/with_menus.rb" }
def menu_item_kind_location?
  has_administration_locations?
end
```

## Cas des éléments nécessitant un target plus précis (page, événement, campus...)
1. Si on doit par la suite sélectionner un sous-élément, il faut ajouter le nouveau kind à la méthode `has_about?` du modèle Communication::Website::Menu::Item
2. Ensuite fans le formulaire d'édition de l'item (app/views/admin/communication/websites/menus/items/_form.html.erb) il faut ajouter la collection des objets possibles
3. Enfin dans le javascript qui gère le changement de destination de l'élement de menu il faut ajouter également le cas (app/views/admin/communication/websites/menus/items/kind_switch.js.erb)

## À propos des dépendances et références
Dans les menu item on n'a pas besoin de se soucier des dépendances ni des références.  
Comme on ne peut ajouter comme élément de menu que des éléments déjà connectés à un website on devrait retrouver le target dans le repo Git.  
Ensuite si on déconnecte l'élément (par exemple un diplôme ou un campus) du website, l'élément sera nettoyé par d'autres biais. L'entrée du menu demeurera (jusqu'à ce qu'elle soit supprimée/remplacée manuellement), mais l'entrée sera ignorée dans le thème et donc pas affichée.  

## Exemple complet
Pour un exemple complet d'ajout d'un élément indirect au menu, se référer à cette [PR](https://github.com/osunyorg/admin/pull/1754)