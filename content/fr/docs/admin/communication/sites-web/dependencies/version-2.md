---
title: Version 2
description: Implémentée en 2023
---

Le point de départ de cette version est multiple :
- résoudre les boucles infinies
- permettre l'indirect non listé
- simplifier la maintenance


## Les connexions
L'intuition est de ne pas lister à la volée, mais d'inscrire les dépendances en base de données.

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

Pour matérialiser cela, il faut créer une connexion pour chaque niveau, dans le cas précédent, pour la photo (qui est un blob) :
- object: photo, source: école 
- object: photo, source: formation 
- object: photo, source: enseignant

Si l'école est déconnectée du site, le lien avec les formations va disparaître, donc celui avec les enseignants, donc celui avec la photo.
Ce calcul se fait en nettoyage nocturne pour permettre de reconstruire par le bas, depuis le site Web, ce qui est plus long mais plus fiable.

## L'algorithme

### Principe
Pour éviter la boucle infinie, il faut écrire un algorithme capable de suivre la chaîne de dépendance sans tomber dans la boucle infinie :
- prendre la liste de dépendance directe
- pour chaque dépendance, vérifier si elle est déjà traitée
  - si oui, l'ignorer et ignorer les enfants
  - si non, l'ajouter et reprendre avec les enfants

Une version de l'algorithme qui fait juste les connexions
```ruby
concerns/WithDependencies
  def connect_dependencies(website)
    dependencies.each do |dependency|
      connect_dependency(dependency, website)
    end
  end

  def connect_dependency(dependency, website)
    if website.is_connected?(dependency)
      website.touch_connection dependency
    else
      website.connect dependency
      dependency.connect_dependencies website
    end
  end
```

Une version qui renvoie la liste construite

```ruby
concerns/WithDependencies
  def connect_dependencies(website)
    list = []
    dependencies.each do |dependency|
      list += connect_dependency(dependency, website)
    end
    list
  end

  def connect_dependency(dependency, website)
    list = []
    if website.is_connected?(dependency)
      website.touch_connection dependency
    else
      website.connect dependency
      list += dependency.connect_dependencies website
    end
    lists << website.connection(dependency)
  end
```

```ruby
Communication::Website
  has_many  :connections
  has_many  :objects,
            through: :connections

  def is_connected?(object)
    connection(object).any?
  end

  def connect(object)
    connection(object).first_or_create
  end

  def touch_connection(object)
    connection(object).touch_all
  end

  def disconnect(object)
    connection(object).destroy_all
  end

  protected

  def connection(object)
    connections.where(university: university, object: object)
  end
```

### Le nettoyage nocturne
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