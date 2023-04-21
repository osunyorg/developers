---
title: Itération 4
description: Clarifications, Olivia de Schrödinger et le saumon
---

Le point de départ de cette version reste identique à la version 2:
- résoudre les boucles infinies
- permettre l'indirect non listé
- éviter les commits multiples lors d'une seule action
- simplifier la maintenance

Toute cette démarche est exclusivement liée aux sites Web dans le cadre de l'export des fichiers statiques vers Git.

Il y a 2 natures d'objets :
- les objets directs, dotés d'un website_id (pages, posts, catégories)
- les objets indirects (organisations, personnes, blocs...)

Les dépendances sont les objets nécessaires pour l'affichage de l'objet courant :
- images
- blocs
- personnes listées dans les blocs
- ...
Cette liste est construite récursivement.

Les références sont les objets qui citent (ou peuvent citer) l'objet courant :
- éléments de menu
- parents
- enfants
- blocs de listes
- ...

Les connexions créent des liens entre les objets indirects et les sites Web, par le biais d'un objet direct :
- un bloc par une page
- une personne par le bloc de la page
- une personne par lien direct au site (via la page équipe)
- une école par lien direct au site (about pour un site d'école)
- une formation par son école
- une formation par lien direct au site (about pour un site de formation)
- ...
L'objet direct s'appelle "source“.
Un objet peut être connecté par plusieurs sources.

Attention au cas de la dénormalisation des websites dans les blocs : il faut choisir si on considère le bloc comme un objet direct ou pas.

## Les dépendances

### Principe

Pour éviter la boucle infinie, il faut écrire un algorithme capable de suivre la chaîne de dépendance sans tomber dans la boucle infinie :
- pour chaque dépendance d'affichage, vérifier si elle est déjà traitée
  - si non, l'ajouter et reprendre avec les enfants
  - si oui, l'ignorer et ignorer les enfants
- pour chaque dépendance de référence, vérifier si elle est déjà traitée
  - si non, l'ajouter
  - si oui, l'ignorer

Pour respecter les principes de [responsabilité unique](https://en.wikipedia.org/wiki/Single-responsibility_principle) et d'[encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)), cet algorithme :
- ne doit pas se préoccuper de site Web (pas de contexte)
- ne doit pas fouiller dans ses propres dépendances (chacun s'occupe de son niveau)

On ne déclare jamais l'objet courant dans ses dépendances (pas d'auto-dépendance).

#### Syncable

La liste de dépendance sert de base à :
- l'établissement des connexions
- l'envoi sur Git

Pour lister les connexions, on ne se préoccupe ni d'état d'activité, ni de publication. 
On liste tout ce qui est connecté à un site, qu'il faille l'envoyer en ligne ou pas encore.

Pour l'envoi sur Git, en revanche, il faut trier ce qui est à envoyer maintenant de ce qui ne l'est pas.
Chaque objet doit donc être capable de répondre à la question `syncable?`, qui signifie "dois-je être envoyé en ligne ?", "dois-je être synchronisé ?".

La méthode récursive doit donc être capable de fonctionner de 2 façons :
- la liste complète (fonctionnement par défaut)
- la liste des objets à envoyer (`syncable_only: true`)

Cela permet de lister les dépendances à ne pas synchroniser, par soustraction.
### Implémentation

```ruby
# Les objets ont souvent besoin de WithGit et WithDependencies, mais pas toujours :
# - les blocks ont des dépendances, mais ne sont pas envoyés sur Git en tant qu'objets, ils passent par leur 'about'
# - les menu items passent par le menu
# - les templates et les components de blocks passent par les blocks qui passent par les 'about'
module WithDependencies
  extend ActiveSupport::Concern

  # Cette méthode doit être définie dans chaque objet,
  # et renvoyer un tableau de ses références directes.
  # Jamais de référence indirecte !
  # Elles sont gérées récursivement.
  def dependencies
    []
  end

  # Method is often overriden
  def syncable?
    if respond_to? :published_now?
      published_now?
    elsif respond_to? :published
      published
    else
      true
    end
  end

  def recursive_dependencies(array: [], syncable_only: false)
    # On renvoie l'array tel quel, non modifié, si on demande les contenus syncable_only et que le contenu ne l'est pas
    return array unless dependency_should_be_added?(array, self, syncable_only)
    dependencies.each do |dependency|
      # Si l'objet ne doit pas être ajouté on n'ajoute pas non plus ses dépendances récursives
      # C'est le fait de couper ici qui évite la boucle infinie
      next unless dependency_should_be_added?(array, dependency, syncable_only)
      array << dependency
      next unless dependency.respond_to?(:recursive_dependencies)
      array = dependency.recursive_dependencies(array: array, syncable_only: syncable_only)
    end
    array.compact
  end

  def recursive_dependencies_unsyncable
    @recursive_dependencies_unsyncable ||= recursive_dependencies - recursive_dependencies(syncable_only: true)
  end

  protected
  
  def dependency_should_be_added?(array, dependency, syncable_only)
    # Si l'objet est déjà là, on ne doit pas l'ajouter
    # Si l'objet n'est pas syncable, on ne doit pas l'ajouter non plus
    !dependency.in?(array) && dependency_should_be_synced?(dependency, syncable_only)
  end
  
  def dependency_should_be_synced?(dependency, syncable_only)
    # Si on n'est pas en syncable only on liste tout, sinon, il faut analyser
    !syncable_only || (dependency.respond_to?(:syncable?) && dependency.syncable?)
  end

end
```

Warning : vérifier que `dependency.in?(list)` ne donne pas de faux négatifs à cause d'active record.
Par exemple, j'instancie 2 fois le même auteur, mais les objets sont distincts en RAM, donc interprétés comme différents.
A mettre sous test.

### Exemple pour une page
```ruby
Communication::Website::Page
  include WithDependencies

  def dependencies
    # Les images à la une, héritées ou pas
    active_storage_blobs + 
    # Les blocks (pas besoin de lister les dépendances des blocs, c'est récursif)
    blocks 
  end
```

La définition des dépendances est particulièrement délicate. 
Il faut être strict, sinon on arrive vite à mettre tout en dépendance de tout.
La question de la nécessité pour l'affichage doit guider les choix.

Quelques exemples...
- Le `parent` n'est pas une dépendance d'affichage, parce qu'il n'est pas nécessaire pour afficher la page.
- Les enfants `children` ne sont pas des dépendances, parce qu'ils ne sont pas nécessaires pour afficher la page.
En revanche, si un bloc "Pages" mentionne des pages, elles sont des dépendances parce qu'il faut les envoyer pour afficher la page complètement.
- Si on a le parent et les enfants, en fait toutes les pages sont reliées entre elles.

## Les références

### Principe

Lister tous les objets qui citent l'objet courant, pour pouvoir les mettre à jour si l'objet change de path par exemple.
À l'idéal, seules certaines opérations déclenchent la mise à jour des références.
Dans un premier temps, on se préoccupe uniquement de lister tout correctement, pas d'optimiser.

### Implémentation

```ruby
concerns/WithReferences

  def references
    []
  end
```

### Exemple pour une page

```ruby
Communication::Website::Page
  include WithReferences

  def references
    # Les items de menu liés à cette page
    menu_items +
    # Les enfants en cas de changement de path
    descendants +
    # Le parent (pour lister les enfants)
    [parent]
  end
```

## Les connexions

### Principe

Tous les objets indirects génèrent une connexion quand ils sont ajoutés aux dépendances d'un objet direct.
Cela permet de répondre aux questions, au sujet d'un objet indirect (personne, organisation...) : 
- dans quels sites l'objet est-il utilisé ?
- dans quel objet direct de ce site l'objet est-il utilisé (quelle page ? quelle actu ?)
- quels objets indirects sont connectés à un site ?

Quand on sauve un objet indirect, il faut faire 3 actions :
- lister les sources et s'y connecter
- connecter toutes les dépendances à toutes les sources
- réenregistrer toutes les sources (donc de tous les websites)

Pour lister les sources, on passe par les connexions, ce qui pose un problème d'œuf et de poule.
Les sources obtenues par les connexions sont les poules, la création d'une connexion est un œuf.
Pour créer l'œuf, il faut itérer récursivement sur les références pour trouver tous les objets directs.
Ensuite, il faut créer des connexions avec chaque object direct.
C'est un système viral : un object connecté connecte ses dépendances et ses références.

Ci-dessous, deux exemples :
- Objet direct : Un bloc créé dans un objet direct, cela crée une connexion via cet objet direct.
- Object indirect : Un bloc créé dans un objet indirect, cela crée une connexion pour chaque objet direct ayant une connexion avec cet objet indirect. Ici, une personne connectée via la page des personnes et via la page des organisations.

![Connexions Osuny](https://user-images.githubusercontent.com/7761386/230399347-c43811a2-11b5-4aef-9535-ca250ca4dc5f.png)

Les connexions sont établies peu importe si l'object indirect doit être envoyé sur Git ou non. 
Par exemple, un Communication::Block d'une page peut être connecté à un site par cette page mais il sera envoyé sur Git selon l'état de publication du bloc (`block.published?`).

### Implementation

```
Communication::Website::Connection
#  id                  :uuid             not null, primary key
#  university_id       :uuid             not null, indexed
#  website_id          :uuid             not null, indexed
#  object_type         :string           indexed => [object_id]
#  object_id           :uuid             indexed => [object_type]
#  source_type         :string           indexed => [source_id]
#  source_id           :uuid             indexed => [source_type]
#  created_at          :datetime         not null
#  updated_at          :datetime         not null
```

On inscrit la connexion dans l'université et dans le site Web.
On mentionne l'objet polymorphe (page, personne, blob...).
Les connexions permettent de retrouver dans quel site est utilisé un objet.

```ruby
module Communication::Website::WithConnections
  extend ActiveSupport::Concern

  included do
    has_many  :connections

    after_save :clean_connections!
  end

  def clean_connections!
    start = Time.now
    connect self
    connections.where('updated_at < ?', start).destroy_all
  end

  def connect(object, source)
    connect_object object, source
    object.recursive_dependencies.each do |dependency|
      connect_object dependency, source
    end
  end

  def disconnect(object, source)
    disconnect_object object, source
  end

  protected

  def connect_object(object, source)
    connection = connections.where(university: university, object: object, source: source).first_or_create
    connection.touch if connection.persisted?
  end

  def disconnect_object(object, source)
    connections.where(university: university, object: object, source: source).destroy_all
  end
```

```ruby
Communication::Website
  WithDependencies
  WithConnections
  
  def dependencies
    pages +
    posts + 
    categories +
    menus +
    [about]
  end
```

## Suppressions

### Principe

Il s'agit de nettoyer les connexions et le référentiel Git.

Plusieurs cas très différents :
1. la suppression d'un objet direct
2. la dépublication d'un objet direct
3. la suppression d'un objet indirect
4. la dépublication d'un objet indirect
5. la déconnexion d'un objet indirect

### 1. Suppression d'objet direct
Exemple: post, page

Il faut simplement supprimer l'objet de la base de données et de git.
Pour autant, on ne peut pas supprimer automatiquement ses dépendances, parce qu'un objet indirect peut rester connecté par le biais d'un autre objet direct.

Difficulté : il s'agit de ne pas en faire trop, d'éviter les suppressions excessives.

Exemple :
Soit une personne connectée 1 fois en explicite et 1 fois par un bloc dans une page. 
Si on supprime la page, la personne ne doit pas être supprimée parce qu'elle reste connectée explicitement.

#### Algorithme
1. Marquer l'objet direct comme à supprimer du référentiel git
2. Pour chaque connexion avec un indirect dont l'objet direct est la source :
  - a. regarder si l'indirect dispose d'une autre connexion
  - b1. si oui, on ne lui fait rien
  - b2. si non, le marquer comme à supprimer du référentiel git
  - c. supprimer la connexion
3. Supprimer du référentiel git tous les objets marqués comme à supprimer
4. Supprimer l'objet de la base de données

### 2. Dépublication d'objet direct
Exemple: post

Il faut "juste" supprimer l'objet de git, en le laissant dans la base de données.
La difficulté est la même que pour le cas 1.

#### Algorithme
1. Marquer l'objet direct comme à supprimer du référentiel git
2. Pour chaque connexion avec un indirect dont l'objet direct est la source :
  - a. regarder si l'indirect dispose d'une autre connexion
  - b1. si oui, on ne lui fait rien
  - b2. si non, le marquer comme à supprimer du référentiel git
3. Supprimer du référentiel git tous les objets marqués comme à supprimer

Les différences par rapport au cas 1 sont :
- on ne supprime pas les connexions (parce que les objets sont toujours connectés au site, même s'il ne faut pas les publier sur git)
- on ne supprimer pas de la base de données (parce que c'est juste un brouillon)

### 3. Suppression d'objet indirect
Exemple: person, program, blob

L'idée simple est de supprimer l'objet concerné.
La subtilité est liée aux dépendances de l'objet : certaines connexions doivent être supprimées, et d'autres pas, comme pour le cas 1.

L'objet indirect peut être connecté à plusieurs sites.
Dans chaque site, les connexions liées à l'objet supprimé doivent être nettoyées.
Comme les connexions peuvent être croisées et récursives, on ne trace pas le détail des connexions parentes.
On sait juste qu'un objet indirect est connecté à un site par un objet direct.
Pour nettoyer, il faut donc reconstruire les connexions de chaque objet direct connecté, en passant par les dépendances, et en supprimant les connexions obsolètes.

#### Algorithme
1. Marquer l'objet indirect comme à supprimer du référentiel git de chacun de ses websites
2. Pour chaque connexion avec un objet direct dont l'objet indirect est le sujet :
  - a. reconstruire les connexions de l'objet direct
  - b. pour toutes les connexions obsolètes (objets indirects précédemments connectés mais qui ne doivent plus l'être)
    - I. regarder si l'indirect dispose d'une autre connexion dans le même site
    - II1. si oui, on ne lui fait rien
    - II2. si non, le marquer comme à supprimer du référentiel git du website de la connexion
    - III. supprimer la connexion
3. Supprimer de chaque référentiel git tous les objets marqués comme à supprimer
4. Supprimer l'objet de la base de données

### 4. Dépublication d'objet indirect
Exemple: block, person, program

Comme pour le cas 2, on supprime "juste" l'objet de git, en le laissant dans la bas de données.

Dans l'état d'Osuny avant cette version 4, l'algo était :
- Quand on dépublie un bloc qui liste une personne, on regarde dans tous les objets des websites pour vérifier que la personne est encore utilisée via une parentèle d'objets synchronisables
  - Si c'était le cas, on ne faisait rien
  - Si ce n'était pas le cas, on supprimait la personne du référentiel Git

Avec les connexions, on simplifie grandement ce calcul.

#### Algorithme (faux)

Pour chaque website de l'objet indirect (via ses connexions) :
1. Marquer l'objet indirect comme à supprimer du référentiel git du website
2. Pour chaque connexion du website avec un objet direct dont l'objet indirect est le sujet :
    - I. regarder si l'indirect dispose d'une autre connexion dans le même site
    - II1. si oui, on ne lui fait rien
    - II2. si non, le marquer comme à supprimer du référentiel git du website de la connexion
3. Supprimer de chaque référentiel git tous les objets marqués comme à supprimer

Les différences par rapport au cas 3 sont :
- on ne supprime pas les connexions (parce que les objets sont toujours connectés au site, même s'il ne faut pas les publier sur git)
- on ne supprimer pas de la base de données (parce que c'est juste un brouillon)

Cet algo est faux parce qu'il ne prend pas en compte les états de publication.

#### Olivia de Schrödinger et le saumon

Exemple :
- Olivia est connectée via Pierre-André (person.blocks) qui est connecté via une formation (program.blocks)
- Olivia est aussi connectée via noesya (organization.blocks) qui est connectée via une formation (program.blocks)
- Pierre-André est dépublié, tandis que noesya est publiée

Par ce principe, en redescendant dans les connexions, Olivia est à la fois dépubliée (par Pierre-André) et publiée (par Noesya), Schrödinger effect.

Pour savoir si Olivia est publiée ou pas, il faut remonter toutes les voies possibles vers la source, comme un saumon qui remonte le courant.
Si toutes les voies sont asséchées (non publiées), Olivia est non publiée.
Si au moins une voie est utilisable (toute la chaîne d'objets est publiée), Olivia est publiée.

L'objectif est de remonter depuis Olivia jusqu'au site en cherchant une séquence de connexions actives, c'est à dire des objets synchronisables.

#### Logique (bonne)

Après exploration (dans la version 5) nous ne trouvons pas de solutions pour remonter la rivière.
Il faut partir de la source (le site Web) et explorer toutes les voies (par les recursive_dependencies avec syncable true).
Cela nous la liste de tous les objets publiés.
Pour obtenir la liste des objets qui ne le sont plus, il faut soustraire.


### 5. Déconnexion d'objet indirect
Exemple: 
- dans un bloc "personnes", suppression d'une personne listée
- déconnexion explicite d'une personne depuis la page "Equipe" dans l'arborescence

En revanche, quand on déconnecte un objet indirect, il faut reconstruire les connexions des sources.

En fonction de la rapidité d'exécution, cela se fera soit : 
- en synchrone (peu probable)
- en asynchrone immédiat
- en asynchrone nocturne
