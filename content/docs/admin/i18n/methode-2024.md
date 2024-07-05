---
title: Méthode 2024
weight: 1
---
{{< callout type="info" >}}
Méthode appliquée à partir de juillet 2024.
{{< /callout >}}

## Principe

L'ancienne méthode s'appuie sur des cercles d'objets, dont l'un est l'original et l'autre les traductions.
Ainsi, un objet qui existe en 3 langues est en fait une collection de 3 objets, dont l'un des 3 est l'original.
Cela pose des problèmes pour gérer les liens entre objets, parce qu'il faut créer des liens entre objets du même espace linguistique.
Quand on supprime un lien, faut-il le supprimer partout ? 
Quand on crée une nouvelle traduction, faut-il créer automatiquement les traductions des objets liés ?

La nouvelle méthode sépare les contenus traduits des objets eux-mêmes.
Un objet `Localization` est ajouté pour chaque objet, nommé `about`.
Ainsi, une organisation a des localisations.

Les blocs eux-mêmes sont attachés aux localisations, et plus aux objets.
On a donc un ensemble d'objets reliés entre eux, comme peuvent l'être un article et son auteur, et pour chacun de ces objets, des localisations.

Certaines données ne sont pas localisables, comme par exemple les coordonnées géographiques d'une organisation, ou sa ville.
Le fait de traduire une organisation ne change pas son siège social.
Il faut donc définir, pour chaque objet, ce qui se traduit ou pas.
C'est en réalité assez subtil, donc ce sera documenté ci-dessous.

## Implémentation

### Routes

### Modèles

### Controlleurs

### Vues

## Migration

Toutes les informations localisées sont supprimées de l'organisation elle-même, et déplacées dans la localisation.
Pour gérer la migration, cela se fait en 2 passes, d'abord le déplacement, puis la suppression.

``` ruby {filename="migration"}
  create_table :university_organization_localizations, id: :uuid do |t|
    t.string  :address_additional
    t.string  :address_name
    t.string  :linkedin
    t.string  :long_name
    t.string  :mastodon
    t.text    :meta_description
    t.string  :name
    t.string  :slug
    t.text    :summary
    t.text    :text
    t.string  :twitter
    t.string  :url

    t.references :about, foreign_key: { to_table: :university_organization }, type: :uuid
    t.references :language, foreign_key: true, type: :uuid
    t.references :university, foreign_key: true, type: :uuid

    t.timestamps
  end

  University::Organization.find_each do |orga|
    # If "old way" translation, we set the about to the original, else if "old way" master, we take its ID.
    about_id = orga.original_id || orga.id

    l10n = University::Organization::Localization.create(
      address_additional: orga.address_additional,
      address_name: orga.address_name,
      linkedin: orga.linkedin,
      long_name: orga.long_name,
      mastodon: orga.mastodon,
      meta_description: orga.meta_description,
      name: orga.name,
      summary: orga.summary,
      slug: orga.slug,
      text: orga.text,
      twitter: orga.twitter,
      url: orga.url,
      about_id: about_id,
      language_id: orga.language_id,
      university_id: orga.university_id
    )

    # Copy from orga (old) to localization (new)
    orga.translate_contents!(l10n)
    orga.translate_other_attachments(l10n)

    # Get permalinks (for aliases)
    orga.permalinks.each do |permalink|
      new_permalink = permalink.dup
      new_permalink.target = l10n
      new_permalink.save
    end

    l10n.save
  end
```

### Organization

Propriétés localisées 

| Propriété | Explication |
|-|-|
| address_additional | Cette propriété est très versatile, mais on peut imaginer traduire "Bureau 508" |
| address_name | "Head office", "Siège social"... |
| linkedin | Les urls sont différentes en fonction des langues |
| long_name | "Groupe d'experts intergouvernemental sur l'évolution du climat", "Intergovernmental Panel on Climate Change" |
| mastodon | Les comptes peuvent être différents pour chaque langue |
| meta_description | La description de page est dans la langue |
| name | "GIEC", "IPCC" |
| summary | Le résumé est dans la langue |
| slug | Le slug dépend du nom |
| text | La présentation de l'organisation est localisée|
| twitter | Les comptes peuvent être différents pour chaque langue |
| url | Les urls peuvent être distinctes, qu'il s'agissent de domaines différents ou d'un /fr /en |
| logo | Le logo peut être complètement différent (si le nom l'est) ou juste avoir des variantes locales |
| logo_on_dark_background | Idem logo |

Propriétés non localisées 

| Propriété | Explication |
|-|-|
| active | Pas évident du tout : faut-il pouvoir désactiver l'orga entière ? Ou bien les locas ? |
| address | L'adresse est identique quelle que soit la langue |
| city | La ville est identique quelle que soit la langue |
| country | Le pays est un code (FR, IT), donc neutre linguistiquement |
| email | Pas évident du tout : pourquoi un mail unique ? |
| kind | Le type (asso, organisation publique) ne dépend pas de la langue, même si la traduction peut varier :ASBL en Belgique, association loi 1901 en France, non-profit au UK... |
| latitude | La latitude ne change pas en fonction de la langue |
| longitude | La longitude ne change pas en fonction de la langue |
| nic | Cette propriété est probablement inutilisée, c'est un ajout au SIREN pour composer le SIRET |
| phone | Pas évident du tout : il pourrait y avoir un numéro par langue |
| siren | Siren et NIC sont trop français, il faudrait un nom de propriété international. Quoi qu'il en soit, ça ne change pas en fonction de la langue. |
| zipcode | La traduction ne change rien au code postal |

### Post