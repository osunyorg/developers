---
title: Version 3
description: Pas encore implémentée
---

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
Ce calcul se fait en nettoyage nocturne pour permettre de reconstruire par le bas, depuis le site Web, ce qui est plus long mais plus fiable.### Le contexte
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

En effet, dans l'exemple où une personne A est connecté via une organisation A, si on déconnecte l'organisation A, la personne A doit aussi être déconnectée, sinon elle va reconnecter l'organisation via ses dépendances. Cela évite de rester bloqué dans des boucles logiques.
Ainsi, dans le cas où la personne A est aussi connectée via une organisation B, elle sera connectée 2 fois (via les organisations A et B). Si l'organisation A est déconnectée, la personne gardera sa connexion au site via l'organisation B.

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
