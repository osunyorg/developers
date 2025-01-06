---
title: Médiathèque
---

Une médiathèque ou bibliothèque de médias est un espace dans lequel on peut accéder à l'ensemble des médias importés dans un système. Le terme “médias” concerne les images (png, jpg, svg), mais aussi les fichiers joints ajoutés dans les blocs de contenus. 

La demande d’une bibliothèque de médias est systématique de la part des utilisateurs et utilisatrices de WordPress, qui ne conçoivent pas de travailler sans. Une part du besoin vient du fait que WordPress impose souvent de réutiliser les mêmes éléments à plusieurs endroits. Ainsi, on peut être amené à utiliser un visuel deux fois : une fois pour illustrer une page, et une seconde fois pour mentionner la page avec une illustration et un lien au sein d'une autre page. Dans Osuny, beaucoup de choses fonctionnent par référence. Ainsi, quand on utilise un bloc “Actualités” pour lister des actus, on réutilise automatiquement les images à la une et les résumés de chaque actu. Il suffit de les modifier à la source pour que toutes les occurrences se mettent à jour automatiquement. 

Néanmoins, il y a deux cas qui rendent la bibliothèque de médias pertinente, ce qui nous amène à envisager son développement. Le premier cas, c’est l’intégration d’une photothèque spécifique, avec des images dédiées. Osuny intègre de série les photothèques Unsplash et Pexels, avec une gestion des droits conforme. La possibilité d’une photothèque dédiée permet d’ajouter un ensemble d’images à la direction artistique cohérente, avec les bons lieux, les bons sujets, les bonnes personnes. L’autre cas pertinent, c’est la gestion des droits. Le fait de développer la bibliothèque de médias permettra dans le futur de développer une gestion fine des durées des droits négociés avec les photographes, de prévenir des fins de droits qui approchent et de mettre automatiquement hors ligne les photos dont les droits ont expiré. 

## Usages

### Collections de photos

La possibilité de chercher une photo dans une collection gérée par l'organisation est un cas d'usage très pertinent.

Plusieurs étapes :
1. Créer une collection (les photos des Beaux-Arts de Marseille)
2. Envoyer des photos
3. Les ranger (catégories, crédits, descriptions)
4. Chercher parmi ces photos (par nom, description, catégories) 

Cette recherche concerne les images à la une, mais aussi tous les blocs contenant des images (chapitre, image, galerie, appel à action...).
Une fois choisie, une photo récupère son crédit et sa balise alt, mais elles peuvent être modifiées au cas par cas.

### Alerte au doublon

La médiathèque permet la détection d'un éventuel doublon, et de l'optimiser. Concrètement, il faudrait utiliser le même fichier physique à chaque usage, et éviter les copies qui génèrent des téléchargements multiples.

https://github.com/rails/rails/issues/31674

### Remplacement en masse

Dans le cas, rare mais possible, d'une utilisation multiple d'un fichier, il peut être pratique de remplacer en une fois toutes les occurences.
On peut imaginer le cas d'une plaquette de présentation de formation (envoyée dans la formation elle-même), dont le fichier a été utilisé une seconde fois dans une page d'aide à la candidature. 

### Nettoyage

Si un média n'est plus utilisé, il est bon de pouvoir le supprimer. 
On peut également envisager un nettoyage automatique après une certaine durée d'inutilisation.

### Gestion des droits

Certaines photos ont des droits gérés, avec une durée déterminée. 
La médiathèque peut permettre de saisir la date de fin de droit d'une photo, et l'auteurice de la photo.


Cela permettrait de générer :
- des alertes avant la fin de droit
- une suppression automatique des sites après l'expiration

## La médiathèque Osuny

### Fonctionnalités

Lors de l'import d'un fichier, on doit pouvoir choisir entre :
- l'envoi d'un fichier depuis son ordinateur
- la recherche dans une collection
- la recherche dans Unsplash et Pexels
- la recherche dans la médiathèque hors collections

La médiathèque doit présenter plusieurs sources :
- les collections gérées
- les photos importées d'Unsplash et Pexels
- les fichiers hors collections

Chaque média doit présenter : 
- les informations intrinsèques (nom de fichier, poids, extension, date de création)
- les informations annexes (alt et crédit pour les images, notes privées)
- les usages (liste des différents contextes d'usage, avec liens)

{{< callout type="info" >}}
  Les fichiers d'imports de personnes ou d'organisations ne sont pas considérés comme des médias, et n'apparaissent donc pas dans la bibliothèque.
{{< /callout >}}

### Ergonomie

Une nouvelle partie “Médiathèque” apparaît dans le royaume communication.
Cette partie donne accès aux médias avec un filtre, permet de gérer les collections, les catégories, et bien sûr les médias eux-mêmes.


Dans les contenus susceptibles d'utiliser des médias, le bouton “Chercher une image” présent dans les images à la une devient un bouton “Médiathèque”. On garde la possibilité d'un import direct, qui va incidemment ajouter le fichier aux médias.


Lorsqu'on appuie sur le bouton “Médiathèque”, s'ouvre une modale plein écran qui offre un champ de recherche, des filtres et des sources. 
Cela permet de chercher dans les bases de données Unsplash et Pexels, mais aussi dans la médiathèque de l'instance. 
Si la recherche textuelle est bien adaptée pour de larges bases de données contenant beaucoup de tags, on peut imaginer que la navigation dans la médiathèque fonctionnera davantage par sélection de catégories.

### Lien avec ActiveStorage

L'implémentation de la media library s'appuie de façon aussi légère que possible sur [ActiveStorage](https://guides.rubyonrails.org/active_storage_overview.html), le système natif de gestion des fichiers de Ruby on Rails. 

ActiveStorage s'appuie sur 3 tables.

```ruby
  create_table "active_storage_attachments", id: :uuid do |t|
    t.string "name", null: false
    t.string "record_type", null: false
    t.uuid "record_id", null: false
    t.uuid "blob_id", null: false
    t.datetime "created_at", precision: nil, null: false
    t.index ["blob_id"], name: "index_active_storage_attachments_on_blob_id"
    t.index ["record_type", "record_id", "name", "blob_id"], name: "index_active_storage_attachments_uniqueness", unique: true
  end

  create_table "active_storage_blobs", id: :uuid do |t|
    t.string "key", null: false
    t.string "filename", null: false
    t.string "content_type"
    t.text "metadata"
    t.string "service_name", null: false
    t.bigint "byte_size", null: false
    t.string "checksum"
    t.datetime "created_at", precision: nil, null: false
    t.uuid "university_id"
    t.index ["key"], name: "index_active_storage_blobs_on_key", unique: true
    t.index ["university_id"], name: "index_active_storage_blobs_on_university_id"
  end

  create_table "active_storage_variant_records", id: :uuid do |t|
    t.uuid "blob_id", null: false
    t.string "variation_digest", null: false
    t.index ["blob_id", "variation_digest"], name: "index_active_storage_variant_records_uniqueness", unique: true
  end
```

`ActiveStorage::Blob` représente le fichier lui-même. On relie le Blob à un objet par le biais d'un `ActiveStorage::Attachment`, avec un attribut polymorphe `record`. Dans le contexte d'Osuny, les blobs sont liés à une université, afin de gérer le multi-tenant. La question des variantes n'a pas grande importance pour la médiathèque, bien que chaque variante génère un nouveau blob. Il faut donc ignorer ces blobs de variantes.

Malheureusement, ActiveStorage ne maintient pas l'unicité des Blobs : le même fichier, envoyé 2 fois, générera 2 blobs. 
Par ailleurs, la suppression d'un attachment provoque la suppression de son blob ([sauf s'il est attaché plusieurs fois](https://discuss.rubyonrails.org/t/activestorage-same-file-attached-multiple-times-single-blob-or-multiple-blobs/73028/3)).

{{< callout type="info" >}}
  Soit on compose avec ce fonctionnement, au prix de fichiers multiples, soit on hack ActiveStorage pour arriver à une unicité des Blobs. 
  Nous choisissons de composer avec le fonctionnement natif, dans le but de rester aussi simple que possible.
{{< /callout >}}

### Modèle de données

Pour utiliser le terme média avec le pluriel médias (et pas le medium/media latin), il faut un inflecteur spécifique.

```ruby {filename="config/initializers/inflections.rb"}
inflect.irregular 'media', 'medias'
```

Puis il faut un ensemble de tables pour gérer les médias.

```ruby
  # Medias
  create_table "communication_medias", id: :uuid do |t|
    # Origin
    # 1   file uploaded through content (default)
    # 2   file uploaded through media library
    # 11  Unsplash
    # 12  Pexels
    t.integer "origin", default: 1, null: false
    # Digest::SHA2.hexdigest
    t.string "digest"
    # All variants are ignored
    t.boolean "variant", default: false
    # The original blob, used for media previews
    t.uuid "active_storage_blob_id", null: false
    # Blob content_type
    t.string "content_type"
    # Blob filename
    t.string "filename"
    # Blob size
    t.bigint "byte_size"
    t.uuid "communication_media_collection_id"
    t.datetime "created_at", precision: nil, null: false
    t.uuid "university_id"
  end

  # Localized texts for medias
  create_table "communication_media_localizations", id: :uuid do |t|
    t.string "name"
    t.uuid "about_id"
    t.uuid "language_id"
    t.uuid "university_id"
    t.text "alt"
    t.text "credit"
  end

  # Context to blobs in use (what are they attached to?) 
  # We cannot use attachments, because blocks do not use them, they use blob ids in JSON.
  create_table "communication_media_blobs", id: false do |t|
    t.uuid "active_storage_blob_id", null: false
    t.uuid "communication_media_id", null: false
    t.string "about_type"
    t.uuid "about_id"
  end

  # Collections
  create_table "communication_media_collections", id: :uuid do |t|
    t.string "name", null: false
    t.datetime "created_at", precision: nil, null: false
    t.uuid "university_id"
  end

  # Categories
  create_table "communication_media_categories", id: :uuid do |t|
    t.string "name", null: false
    t.datetime "created_at", precision: nil, null: false
    t.uuid "communication_media_collection_id"
    t.uuid "university_id"
  end

  # Join medias and categories
  create_table "communication_medias_media_categories", id: false, force: :cascade do |t|
    t.uuid "communication_media_id", null: false
    t.uuid "communication_media_category_id", null: false
  end
```

### Implémentation

Il faut s'intercaler dans le processus de création des blobs et des variantes pour créer les médias.
Il ne faut pas créer trop de médias (les variantes n'en sont pas).
Il ne faut pas non plus ralentir le processus, donc il faut travailler en arrière plan.

#### Approche 1 

Hooker la création des blobs et des variantes.
Pour cela, il faut ajouter des actions à la création des objets, via l'initializer.

```ruby{filename="config/initializers/active_storage.rb"}
Rails.application.config.to_prepare do
  module ActiveStorageMediaLibraryBlob
    extend ActiveSupport::Concern

    included do
      after_commit :add_to_media_library
    end

    protected

    def add_to_media_library
      Communication::Media::AnalyzeBlobJob.perform_later(self)
    end
  end

  ActiveStorage::Blob.include ActiveStorageMediaLibraryBlob

  module ActiveStorageMediaLibraryVariant
    extend ActiveSupport::Concern

    included do
      after_commit :add_to_media_library
    end

    protected

    def add_to_media_library
      Communication::Media.discard_variant(self)
    end
  end

  ActiveStorage::VariantRecord.include ActiveStorageMediaLibraryVariant
end
```

En principe, la création de variante vient après la création de blob, mais il y faut vérifier s'il n'y a jamais d'inversion possible.
Lorsqu'on analyse le média, on calcule son digest et on crée le média avec s'il n'existe pas.


Après implémentation, cette approche est une fausse piste : on ne dispose pas du contexte de création.

#### Approche 2

Il faut, à chaque endroit où l'on crée et utilise des médias, intercaler une action de création du média ou de contexte d'utilisation.

