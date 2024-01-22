---
title: Multilingue
weight: 3
description: >
  Parti-pris sur la gestion des langues
---

D'une façon générale, les langues se gèrent en 2 phases :
- l'internationalisation (i18n) qui vise à rendre possible les langues multiples
- la localisation (l10n) qui vise à intégrer une langue spécifique

Dans Osuny, le système a 2 facettes :
- l'admin en Ruby on Rails, qui permet d'écrire les contenus
- le site en Hugo, qui permet de lire les contenus

L'aspect front du multilingue [est documenté dans la partie thème](/docs/theme/architecture/multilingue/).  
  
Dans l'admin d'Osuny il y a deux grands types d'objets : 
- les objets directs (qui ont une dépendance directe à un site web), comme les pages, les actualités...
- les objets indirects (ceux qui sont "neutres", non connectés à un site), comme une personne, une organisation...

## Cas des objets indirects

### Setup des langues disponibles globalement
Osuny dispose d'un certain nombre de langues inclues (modèle `Language`). Le choix et l'ajout de langues se fait dans la partie `Server` d'Osuny. Il sera par la suite possible de traduire tous les objets indirects dans ces différentes langues. A la création d'un site web on pourra également choisir d'activer une ou plusieurs de ces langues.  
Lorsqu'on crée une université depuis la partie Server on doit choisir laquelle (parmi ces langues) doit être la langue par défaut de l'université. Chaque nouvel objet indirect créé par la suite sera considéré comme créé dans cette langue. Par exemple si on choisit le français comme langue par défaut de l'université X, toutes les fiches d'organisations créées seront par défaut en français.

### Création / traduction d'un nouvel objet
Les objects indirects peuvent (toujours) être traduits dans toutes les langues disponibles sur la plateforme Osuny.  
Quand un objet indirect est créé il prend comme langue la langue par défaut de l'université, et on peut ensuite le traduire dans toutes les autres langues disponibles globalement dans Osuny.

## Cas des objets directs

### Setup des langues disponibles pour un site web
À la création d'un site web on force le choix d'au moins une langue. On peut en activer plus d'une, parmi les différentes langues disponibles. Si on en active plusieurs les contenus vont alors devenir traduisibles. On doit également choisir la langue par défaut su site web.

### Création / traduction d'un nouvel objet
Si un site web dispose de plusieurs langues actives alors chaque nouvel object créé sera créé dans la langue par défaut du site web, et ensuite on pourra le traduire dans les autres langues disponibles pour le site web.

## Rendre un objet traduisible
1. Ajouter les propriétés au modèle
Pour qu'un objet devienne traduisible il faut lui rajouter plusieurs propriétés :
- language_id (référence vers sa langue)
- original_id (référence vers son "master", l'objet lié créé dans la langue par défaut)

2. Ajouter les méthodes nécessaires au modèle
Ensuite il faut lui inclure le concern `WithTranslations` qui va s'occuper d'établir les relations (belongs_to :language, ...). Il ajoute également des scopes sur l'objet : `for_language` et `for_language_id`. Il y a aussi tout un tas de méthodes, notamment des fonctions pour créer les translations.

La suite dépend du type d'objet.

### Object directs

3. Ajouter un swith de langue dans la vue de l'objet
On est au niveau d'un objet direct, et donc scopé dans un website. On bénéficie du menu global du site web à gauche, qui inclut un switch de langue. En gros ce switch de langue affiche toutes les langues disponibles pour le site web et créé des liens vers l'url courantes en injectant un paramètre lang=iso_code. 
La langue en cours est déterminée grâce au helper `current_website_language`.

4. Ajouter le concern au niveau du controller
Il faut inclure le concern `include Admin::Translatable` au niveau du controller de l'objet visé. A chaque fois qu'on a besoin de charger un objet (show, edit, update, destroy), ce concern va automatiquement vérifier si on est bien sur la bonne traduction de l'objet, ou la créé à la volée si elle n'existe pas encore.  
Il ne faut pas oublier, lorsqu'on charge une liste d'objets ou un objet, de scoper à la langue en cours (`Page.for_language(current_website_language)`).

### Objects indirects

3. Ajouter la route
Il faut ajouter une route à l'objet :

```
resources **object** do
    member do
      get "/translations/:lang" => "**object**#in_language", as: :show_in_language
    end
  end
```
(le nommage de la route en `show_in_language` est important).

4. Ajouter un switch de langue dans la vue de l'objet
Dans chaque vue (a priori dans le show) d'objet multilingue on a besoin d'avoir un switch de langue. On va donc forcer le render d'un partial commun :
```
render 'admin/application/i18n/widget', about: **object**
```
Ca va ajouter un choix de langue qui renvoie sur la route `show_in_language` de l'objet.  
La langue en cours est déterminée en comparant avec la langue de l'objet affichée.

5. Gestion du controller

Il faut créer dans le controller une méthode correspondant à la route créée en step 3.

```
def in_language
    language = Language.find_by!(iso_code: params[:lang])
    translation = @**object**.find_or_translate!(language)
    if translation.newly_translated
      # There's an attribute accessor named "newly_translated" that we set to true
      # when we just created the translation. We use it to redirect to the form instead of the show.
      redirect_to [:edit, :admin, translation.becomes(translation.class.base_class)]
    else
      redirect_to [:admin, translation.becomes(translation.class.base_class)]
    end
  end
```

## L'export statique

En cas de site monolingue les `url` des fichiers statiques ne doivent PAS être préfixées de la langue.  
En cas de multilingue toutes les urls sont préfixées.  
On créé les contenus dans `content/:lang/`.
On crée les menus dans `data/menus/:lang/`.  


## A noter
Dans l'admin d'Osuny, l'affichage des noms de langues est géré par la gem `i18n_data`. On a créé une méthode helper (dans app/helpers/application_helper.rb) nommée `language_name` qui va chercher le nom de la langue en fonction du code ISO. On utilise ce helper à chaque fois qu'on a besoin d'afficher un nom de langue.