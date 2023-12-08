---
title: Catégories
---

Les actualités (posts) et les événements de l'agenda (events) utilisent un système de catégories.
Ce système permet de regrouper les sujets similaires.

Exemples:
- https://ran-coper.fr/toutes-les-actualites/eco-construction/
- https://www.osuny.org/actualites/veille/
- https://www.communication-democratie.org/fr/publications/enjeux/consommation/

## Structure des URLs

Prenons l'exemple des styles musicaux (jazz, rock...).

### Catégories multi-entités

```text {filename="URL"}
/categories
```

- Liste de toutes les catégories, non paginée.
- Si les catégories ont des enfants, liste des catégories racine (sans parent).

### Catégorie multi-entités

```text {filename="URL"}
/categories/jazz
```

Liste des objets récents de la catégorie. 
- Les événements à venir liés au jazz
- Un lien vers les événements de la catégorie
- Les n articles récents liés au jazz
- Un lien vers les articles liés au jazz
- Liste des catégories enfants de jazz (be-bop, free, manouche...)

### Liste des actualités d'une catégorie

```text {filename="URL"}
/actualites/jazz
```

- Liste des actualités liées à la catégorie jazz.
- Liste des catégories enfants de jazz

### Page 2 de la liste des actualités d'une catégorie

```text {filename="URL"}
/actualites/jazz/page/2
```

- Seconde page des actualités liées à la catégorie jazz.

### Liste des événéments d'une catégorie

```text {filename="URL"}
/agenda/jazz
```

- Liste des événement à venir liés à la catégorie jazz.
- Liste des catégories enfants de jazz

### Page 2 de la liste des événéments d'une catégorie

```text {filename="URL"}
/agenda/jazz/page/2
```

- Seconde page des événements à venir liées à la catégorie jazz.

### Événements archivés d'une catégorie

```text {filename="URL"}
/agenda/jazz/archives
```

- Liste des événement archivés liés à la catégorie jazz.

### Page 2 des événements archivés d'une catégorie

```text {filename="URL"}
/agenda/jazz/archives/page/2/
```

- Seconde page des événement archivés liés à la catégorie jazz.

## Implémentation

Au 28 novembre 2023, nous avons une taxonomie unique `categories`.
Comment réaliser les URLs souhaitées ?

{{% details title="Analyse Hugo" closed="true" %}}
- https://discourse.gohugo.io/t/per-section-content-filter-using-taxonomy/18172/6
- https://discourse.gohugo.io/t/changing-taxonomy-urls/6023/10
- https://github.com/gohugoio/hugo/issues/1208
- https://discourse.gohugo.io/t/create-section-taxonomies/343/6
- https://www.hannaliebl.com/blog/nesting-taxonomies-in-hugo/
- https://github.com/guayom/hugo-taxonomies
- https://glennmccomb.com/articles/how-to-build-custom-hugo-pagination/
{{% /details %}}

2 approches se dégagent :
1. Soit on a une taxonomie unique, et les objets sont mélangés
2. Soit on a deux taxonomies

## Taxonomie unique

Dans ce cas on n'arrive pas à avoir toutes les urls souhaitées, mais seulement `/categories/jazz`.
Il faut donc graphiquement articuler des actualité et des événements, afin de bénéficier de la pagination native.
Ce n'est pas satisfaisant.

## Taxonomies multiples

### Dans le site

Créer 2 taxonomies :

```yaml {filename="config/_default/taxonomies.yaml"}
posts_category: posts_categories
events_category: events_categories
```

Créer 2 dossiers dans `content` pour ranger les 2 taxonomies, avec un index fixant l'URL et le titre :

{{< filetree/container >}}
  {{< filetree/folder name="content" >}}
    {{< filetree/folder name="fr" state="open" >}}
      {{< filetree/folder name="posts_categories" state="open" >}}
        {{< filetree/folder name="jazz" state="closed" >}}{{< /filetree/folder >}}
        {{< filetree/folder name="rock" state="closed" >}}{{< /filetree/folder >}}
        {{< filetree/file name="_index.html" >}}
      {{< /filetree/folder >}}
      {{< filetree/folder name="events_categories" state="open" >}}
        {{< filetree/folder name="jazz" state="closed" >}}{{< /filetree/folder >}}
        {{< filetree/folder name="rock" state="closed" >}}{{< /filetree/folder >}}
        {{< filetree/file name="_index.html" >}}
      {{< /filetree/folder >}}
    {{< /filetree/folder >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

Dans les fichiers d'index, définir les titres et les URLs :

```yaml {filename="content/fr/posts_categories/_index.html"}
---
title: Catégories d'actualités
url: /fr/actualites/categories
---
```

{{< callout type="warning" >}}
  Les index de catégories `/fr/actualites/categories` et `/fr/agenda/categories` ne sont pas développés (8 décembre 2023), parce que personne ne semble en avoir besoin. 
  Les pages sont disponibles aux URLs par défaut de Hugo, `/fr/posts_categories` et `/fr/events_categories`.
{{< /callout >}}



On obtient les urls `/actualites/jazz` et `/agenda/jazz`.

### Dans le thème

Il faut créer `posts_categories` et `events_categories` à la place de `categories`.

Les partials `/categories` sont peut-être partagés, peut-être pas, à voir.

Il faut reprendre les styles et les exceptions de search, parce que la classe `categories__term` se sépare en 2, `posts_categories__term` et `events_categories__term`.

### Dans l'admin

Deux questions se posent : 
- comment gérer et générer les 2 taxonomies ?
- comment générer les pages multi-entités ?

**Option séparée**
La première possibilité est de ne rien gérer, c'est à dire de créer 2 taxonomies séparées, les catégories d'actualité d'un côté et les catégories d'évnéments de l'autre. 
Si on va dans ce sens, les taxonomies peuvent être différentes, ce qui est pratique dans certains cas (taxos distinctes) et pénible dans d'autres (taxos identiques). 
Pour générer les pages multi-entités, on crée des pages simples et on ajoute un bloc actualités de la catégorie d'actualités, et un bloc agenda de la catégorie d'événements.
C'est rudimentaire, mais souple et efficace.

**Option groupée**
On peut choisir de maintenir un seul objet dans l'admin, et de générer les 3 objets automatiquement : page multi-entités, catégorie d'actualités, catégorie d'événements. 
Ça évite le copier-coller, mais ça empêche de distinguer les taxonomies.
Une question se pose alors en front : que faut-il afficher en premier ?

**Conclusion**
L'option séparée est plus simple, et l'économie de temps en copier-coller ne justifie pas la complexité additionnelle en architecture et en code.

### Dans les blocs

Il faut pouvoir afficher non pas les actualités ou événements d'une catégorie, mais les noms des catégories avec le nombre d'objets dedans.

En suivant l'option séparée, et pour éviter de multiplier les blocs, on peut ajouter une option "Lister les catégories" dans les blocs actualités et agenda.