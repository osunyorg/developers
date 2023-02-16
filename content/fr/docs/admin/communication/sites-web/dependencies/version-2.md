---
title: Version 2
description: Implémentée en 2023
---

Le point de départ de cette version est multiple :
- résoudre les boucles infinies
- permettre l'indirect non listé
- éviter les commits multiples lors d'une seule action
- simplifier la maintenance

Plusieurs intuitions guident cette version :
1. lister les dépendances avec un algorithme récursif qui évite les boucles infinies
2. inscrire les connexions en base de données
3. traiter clairement les liens entre objets et sites Web
4. consolider chaque nuit les connexions (ce qui traite aussi la publication d'actualité dans le futur) 

## 1. La liste de dépendances

### Principe
Pour éviter la boucle infinie, il faut écrire un algorithme capable de suivre la chaîne de dépendance sans tomber dans la boucle infinie :
- prendre la liste des dépendances directes
- pour chaque dépendance, vérifier si elle est déjà traitée
  - si non, l'ajouter et reprendre avec les enfants
  - si oui, l'ignorer et ignorer les enfants

Pour respecter les principes de [responsabilité unique](https://en.wikipedia.org/wiki/Single-responsibility_principle) et d'[encapsulation](https://en.wikipedia.org/wiki/Encapsulation_(computer_programming)), cet algorithme :
- ne doit pas se préoccuper de site Web (pas de contexte)
- ne doit pas fouiller dans ses propres dépendances (chacun s'occupe de son niveau)

### Implémentation

```ruby
concerns/WithDependencies
  # Cette méthode doit être définie dans chaque objet, et renvoyer un tableau de ses références directes
  def direct_dependencies
    []
  end

  def dependencies(list = [])
    direct_dependencies.each do |dependency|
      next if dependency.in?(list)
      list << dependency
      list += dependency.dependencies(list)
    end
    list
  end
```

Warning : vérifier que `dependency.in?(list)` ne donne pas de faux négatifs à cause d'active record.
Par exemple, j'instancie 2 fois le même auteur, mais les objets sont distincts en RAM, donc interprétés comme différents.
A mettre sous test.

### Exemple pour une page
```ruby
Communication::Website::Page
  def direct_dependencies
    # Les images à la une, héritées ou pas
    active_storage_blobs + 
    # Les blocks (pas besoin de lister les dépendances des blocs, c'est récursif)
    blocks + 
    # Les enfants (pas besoin de lister tous les descendants, c'est récursif)
    children + 
    # Les items liés à cette page (pas besoin de lister les menus eux-mêmes, c'est récursif)
    menu_items
  end
```

## 2. Les connexions

### Principe
```
Communication::Website::Connections
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

On note comme source l'entité qui est à l'origine de la connexion.
Dans beaucoup de scénarios, c'est une chaîne :
- un site est lié à une école
- une école a des formations
- une formation a des enseignants
- un enseignant a une photo

Pour matérialiser cela, il faut probablement créer une connexion pour chaque niveau, dans le cas précédent, pour la photo (qui est un blob) :
- object: photo, source: école 
- object: photo, source: formation 
- object: photo, source: enseignant

Si l'école est déconnectée du site, le lien avec les formations va disparaître, donc celui avec les enseignants, donc celui avec la photo.
Ce calcul se fait en nettoyage nocturne pour permettre de reconstruire par le bas, depuis le site Web, ce qui est plus long mais plus fiable.

### Implémentation

```ruby
Communication::Website
  has_many  :connections
  has_many  :objects,
            through: :connections

  def direct_dependencies
    pages +
    posts + 
    categories +
    menus
  end

  def connect_direct_dependencies
    direct_dependencies.each do |dependency|
      connect_dependency dependency
    end
  end

  def connect_dependencies(object, source: nil)
    connect_dependency object, source
    object.dependencies.each do |dependency|
      connect_dependency dependency, object
    end
  end

  def connect_dependency(dependency, source: nil)
    if is_connected?(dependency)
      touch_connection dependency
    else
      connect dependency
      connect_dependencies website, source
    end
  end

  def is_connected?(object, source: nil)
    connection(object, source: nil).any?
  end

  def connect(object, source: nil)
    connection(object, source: nil).first_or_create
  end

  def touch_connection(object, source: nil)
    connection(object, source: nil).touch_all
  end

  def disconnect(object, source: nil)
    connection(object, source: nil).destroy_all
  end

  protected

  def connection(object, source: nil)
    connections.where(university: university, object: object, source: source)
  end
```

## 3. Le lien objet / sites Web

## 4. Le nettoyage nocturne
Quotidiennement, après minuit, on reconstruit les connexions du site Web pour vérifier l'intégrité et réparer d'éventuels problèmes.
Cela permet de traiter la publication des actualités dans le futur, puisque les publications prévues pour le jour concerné seront ajoutées avec leurs connexions.

À la fin du traitement, il faut supprimer les connexions qui n'ont pas été touchées pendant le processus, puisque cela signifie qu'elles sont obsolètes.
C'est un genre de "garbage collector" des connexions, qui évite d'envoyer de vieux objets un jour après leur suppression.

### Enregistrement d'un site Web

1. Prendre la liste des `objects`
2. Exporter le site et la liste complète vers Git

### Enregistrement d'une page

Si la page a une image, soit on liste l'image comme dépendance et l'after save la prend, soit on la connecte explicitement quand elle change.
La première version est plus simple.
La seconde est plus optimisée, on ne touche à rien si ce n'est pas nécessaire, mais il ne faut pas rater d'étape (ajout, modification, suppression).
Avec les héritages d'images (un objet hérite de l'image de son parent), cette seconde approche est nettement plus compliquée à maintenir.

```ruby
Communication::Website::Page
  def git_dependencies(website)
    dependencies = [self] +
                    website.menus +
                    descendants +
                    active_storage_blobs +
                    siblings +
                    git_block_dependencies +
                    type_git_dependencies
    dependencies += [parent] if has_parent?
    dependencies.flatten.compact
  end

  def git_destroy_dependencies(website)
    [self] +
    descendants +
    active_storage_blobs
  end
```



## La traçabilité

Il est possible, mais nettement plus compliqué, de garder trace de l'origine de la connexion, au niveau de la source et du bloc.
Le niveau bloc ne paraît pas indispensable.

```
Communication::Website::Connections
#  id                  :uuid             not null, primary key
#  university_id       :uuid             not null, indexed
#  website_id          :uuid             not null, indexed
#  object_type         :string           indexed => [object_id]
#  object_id           :uuid             indexed => [object_type]
#  block_id            :uuid             indexed
#  source_type         :string           indexed => [source_id]
#  source_id           :uuid             indexed => [source_type]
#  created_at          :datetime         not null
#  updated_at          :datetime         not null
```
### Le contexte
On inscrit la connexion dans l'université et dans le site Web.

### L'objet
On mentionne l'objet polymorphe (page, personne, blob...).
Un objet peut être listé plusieurs fois, une fois par connexion.
Par exemple, si une personne peut être ajoutée directement au site et ajoutée à un bloc dans une page.
La première connexion aura `block` `nil` et le website comme `source`.
La seconde aura une référence au block, et la page comme `source`.

### Le bloc
Le bloc est un lien optionnel, si l'objet est lié dans le cadre d'un bloc.
Les blocs ne doivent pas être considérés comme des dépendances, parce qu'ils n'existent jamais seuls, ils ont toujours un objet parent (page, actu, personne...).
En revanche, il peut être utile de savoir par quel bloc un objet est connecté à sa source.

### La source
La source est l'entité dont l'objet est la dépendance :
- l'image d'une page a pour source la page
- l'image d'une personne a pour source la personne
- la personne mentionnée dans un bloc a pour source l'objet auquel est rattaché le bloc (la page, par exemple)

### L'algorithme

```ruby
concerns/WithDependencies
  def connect_dependencies(website, source: nil, block: nil)
    dependencies.each do |dependency|
      connect_dependency(dependency, website, source: source, block: block)
    end
  end

  def connect_dependency(dependency, website, source: nil, block: nil)
    if website.is_connected?(dependency)
      website.touch_connection dependency
    else
      if dependency.is_a? Communication::Block
        source = dependency.about
        block = dependency
      end
      website.connect dependency, source: source, block: block
      dependency.connect_dependencies website, source: source, block: dependency
    end
  end
```

```ruby
Communication::Website
  has_many  :connections
  has_many  :objects,
            -> { distinct },
            through: :connections

  def is_connected?(object, source: nil, block: nil)
    connection(object, source, block).any?
  end

  def connect(object, source: nil, block: nil)
    connection(object, source, block).first_or_create
  end

  def touch_connection(object, source: nil, block: nil)
    connection(object, source, block).touch_all
  end

  def disconnect(object, source: nil, block: nil)
    connection(object, source, block).destroy_all
  end

  protected

  def connection(object, source: nil, block: nil)
    connections.where university: university,
                      object: object,
                      source: source,
                      block: block
  end
```