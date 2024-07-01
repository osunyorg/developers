---
title: Méthode 2023
weight: 2
---

{{< callout type="info" >}}
Méthode appliquée jusqu'en juin 2024.
{{< /callout >}}

## Principes 

### Langue d'affichage pour les objets indirects

Sur tous (quasiment) les écrans d'index des différents objets indirects (par exemple la liste des `organizations`) on a un système de switch de langues qui permet de switcher entre les différentes langues de l'université. On voit alors la totalité des contenus disponibles dans la langue d'affichage (master ou non), plus celles qui ne sont pas disponibles encore (master dans une autre langue et pas de traduction existante). Cette magie est permise par le scope `in_closest_language_id`. Si on clique sur un contenu dans la langue d'affichage on va comme d'habitude au show de l'objet. Si on clique sur un contenu qui n'existe pas encore dans la langue d'affichage on créé à la volée la traduction de l'objet dans la langue d'affichage.  
Lorsqu'on créé un nouvel objet indirect, il est créé dans la langue d'affichage actuelle.  

### Langue d'affichage pour les objets directs
Pour tous les objets directs, on dispose d'un switch de langue permettant de passer d'une langue du site à un autre. On n'affiche QUE les objets concernés dans la langue d'affichage. Par exemple sur l'index des posts on ne verra que les posts existants dans la langue d'affichage, pas les posts qui ont été créés dans une autre langue et non traduits.  

## Rendre un objet indirect traduisible

{{% steps %}}

### Ajouter les propriétés au modèle

Créer une migration pour ajouter ces propriétés à un objet :
- language_id (référence vers sa langue)
- original_id (référence vers son "master", l'objet lié créé dans la langue par défaut)  
Il convient en général aussi de mettre une langue par défaut sur tous les objets qu'on modifie. Ca donne une migration complète de cet ordre : 
```
def up
  add_reference :**object**, :language, foreign_key: true, type: :uuid
  add_reference :**object**, :original, foreign_key: {to_table: :**object**}, type: :uuid

  **Object**.reset_column_information
  University.all.find_each do |university|
    university.**object**.update_all(language_id: university.default_language_id)
  end
end

def down
  remove_reference :**object**, :language
  remove_reference :**object**, :original
end
```

### Ajouter les méthodes nécessaires au modèle

Inclure dans le modèle le concern `Translatable` qui va s'occuper d'établir les relations (belongs_to :language, ...). Il ajoute également des scopes sur l'objet : `for_language` et `for_language_id` et `in_closest_language_id`. Il y a aussi tout un tas de méthodes, notamment des fonctions pour créer les translations.

### Ajouter/modifier la route  

Il faut ajouter/modifier la route de l'objet :

```
resources **object**, path: '/:lang/**object**'

```

### Ajouter les éléments de langues au controller

Il faut inclure le concern `include Admin::Translatable` au niveau du controller de l'objet visé. A chaque fois qu'on a besoin de charger un objet (show, edit, update, destroy), ce concern va automatiquement vérifier si on est bien sur la bonne traduction de l'objet, ou la créer à la volée si elle n'existe pas encore.  
Dans l'index il faut ajouter le scope `.in_closest_language_id(current_language.id)`.  
Dans le create il faut set la langue par défaut : `**object**.language_id = current_language.id`.

### Ajouter un switch de langue dans la vue d'index de l'objet 

On ajoute dans l'index ce code :
```
<% content_for :title_right do %>
  <%= render 'language_switcher' %>
<% end %>
```
En gros ce switch de langue affiche toutes les langues disponibles pour l'université et créé des liens vers l'url courantes en injectant un paramètre lang=iso_code. 
La langue en cours est déterminée grâce au helper `current_language`.

### Ajouter un switch de langue dans la vue show de l'objet 

On ajoute dans le show ce code :
```
<%= render 'admin/application/i18n/widget', about: **object**, small: true %>
```
Ce widget se charge d'afficher les différentes langues disponibles pour l'université, tout en indiquant quelle est la langue affichée actuellement, les langues dans lesquelles l'objet est déjà traduit, et celles ou elle ne l'est pas. Un clic sur une langue déjà traduite va envoyer vers le show de l'objet dans sa bonne traduction. Un clic sur une langue non encore traduite va entraîner sa traduction immédiate.

### Liens entre modèles

Si l'objet est utilisé comme référence à un autre endroit (par exemple les auteurs de posts, la source d'un bloc, ...), il faut faire attention à deux choses :
- filtrer en affichage la liste d'objet (utiliser le scope `.in_closest_language_id(current_language.id)` sur la collection)
- Comme du coup dans la liste on peut avoir des objets qui ne sont pas dans la langue de l'objet référent (puisque le scope in_closest_language_id renvoie également les masters des autres langues non traduits dans la langue d'affichage), il faut un peu de post traitement. Par exemple pour les auteurs des posts on va avoir :
```
  before_validation :ensure_author_is_in_correct_language

 def ensure_author_is_in_correct_language
    return unless author.present? && author.language_id != language_id
    author_in_correct_language = author.find_or_translate!(language)
    self.author_id = author_in_correct_language.id
  end

```

{{% /steps %}}

## Rendre un objet direct traduisible

{{% steps %}}

### Ajouter les propriétés au modèle

Créer une migration pour ajouter ces propriétés à un objet :
- language_id (référence vers sa langue)
- original_id (référence vers son "master", l'objet lié créé dans la langue par défaut)  
Il convient en général aussi de mettre une langue par défaut sur tous les objets qu'on modifie. Ca donne une migration complète de cet ordre : 
```
def up
  add_reference :**object**, :language, foreign_key: true, type: :uuid
  add_reference :**object**, :original, foreign_key: {to_table: :**object**}, type: :uuid

  **Object**.reset_column_information
  Communication::Website.all.find_each do |website|
    website.**object**.update_all(language_id: website.default_language_id)
  end
end

def down
  remove_reference :**object**, :language
  remove_reference :**object**, :original
end
```

### Ajouter les méthodes nécessaires au modèle

Inclure dans le modèle le concern `Translatable` qui va s'occuper d'établir les relations (belongs_to :language, ...). Il ajoute également des scopes sur l'objet : `for_language` et `for_language_id` et `in_closest_language_id`. Il y a aussi tout un tas de méthodes, notamment des fonctions pour créer les translations.

### Ajouter/modifier la route  

Il faut ajouter/modifier la route de l'objet :

```
resources **object**, path: '/:lang/**object**'

```

### Ajouter les éléments de langues au controller

Il faut inclure le concern `include Admin::Translatable` au niveau du controller de l'objet visé. A chaque fois qu'on a besoin de charger un objet (show, edit, update, destroy), ce concern va automatiquement vérifier si on est bien sur la bonne traduction de l'objet, ou la créer à la volée si elle n'existe pas encore.  
Dans l'index il faut ajouter le scope `.for_language(current_language)`.  
Dans le create il faut set la langue par défaut : `**object**.language_id = current_language.id`.

### Ajouter un switch de langue dans la vue d'index de l'objet 

On ajoute dans l'index ce code :
```
<% content_for :title_right do %>
  <%= render 'language_switcher' %>
<% end %>
```
En gros ce switch de langue affiche toutes les langues disponibles pour l'université et créé des liens vers l'url courantes en injectant un paramètre lang=iso_code. 
La langue en cours est déterminée grâce au helper `current_language`.

### Ajouter un switch de langue dans la vue show de l'objet 

On ajoute dans le show ce code :
```
<%= render 'admin/application/i18n/widget', about: **object**, small: true %>
```
Ce widget se charge d'afficher les différentes langues disponibles pour l'université, tout en indiquant quelle est la langue affichée actuellement, les langues dans lesquelles l'objet est déjà traduit, et celles ou elle ne l'est pas. Un clic sur une langue déjà traduite va envoyer vers le show de l'objet dans sa bonne traduction. Un clic sur une langue non encore traduite va entraîner sa traduction immédiate.

### Liens entre modèles

Si l'objet est utilisé comme référence à un autre endroit (par exemple les auteurs de posts, la source d'un bloc, ...), il faut faire attention à filtrer en affichage la liste d'objet (utiliser le scope `.in_language(current_language)` sur la collection).

{{% /steps %}}

## L'export statique

En cas de site monolingue les `url` des fichiers statiques ne doivent PAS être préfixées de la langue.  
En cas de multilingue toutes les urls sont préfixées.  
On créé les contenus dans `content/:lang/`.
On crée les menus dans `data/menus/:lang/`.  


## A noter
Dans l'admin d'Osuny, l'affichage des noms de langues est géré par la gem `i18n_data`. On a créé une méthode helper (dans app/helpers/application_helper.rb) nommée `language_name` qui va chercher le nom de la langue en fonction du code ISO. On utilise ce helper à chaque fois qu'on a besoin d'afficher un nom de langue.