---
title: Méthode 2024
weight: 1
---
{{< callout type="info" >}}
Méthode appliquée à partir de juillet 2024.
{{< /callout >}}

L'ancienne méthode s'appuie sur des cercles d'objets, dont l'un est l'original et l'autre les traductions.
Ainsi, un objet qui existe en 3 langues est en fait une collection de 3 objets, dont l'un des 3 est l'original.
Cela pose des problèmes pour gérer les liens entre objets, parce qu'il faut créer des liens entre objets du même espace linguistique.
Quand on supprime un lien, faut-il le supprimer partout ? 
Quand on crée une nouvelle traduction, faut-il créer automatiquement les traductions des objets liés ?

La nouvelle méthode sépare les contenus traduits des objets eux-mêmes.
Un objet `Localization` est ajouté pour chaque objet, nommé `about`.
Ainsi, une organisation a des localisations.

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
```

Toutes les informations localisées sont supprimées de l'organisation elle-même, et déplacées dans la localisation
(pour gérer la migration, cela se fait en 2 passes, d'abord le déplacement, puis la suppression).

``` ruby {filename="migration"}
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