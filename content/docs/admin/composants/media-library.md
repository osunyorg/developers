---
title: Médiathèque
---

Une médiathèque ou bibliothèque de médias est un espace dans lequel on peut accéder à l'ensemble des médias importés dans une instance Osuny. Le terme “médias” concerne les images (png, jpg, svg), mais aussi les fichiers joints ajoutés dans les blocs de contenus. 

{{< callout type="info" >}}
  Les fichiers d'imports de personnes ou d'organisations ne sont pas considérés comme des médias, et n'apparaissent donc pas dans la bibliothèque.
{{< /callout >}}


La demande d’une bibliothèque de médias est systématique de la part des utilisateurs et utilisatrices de WordPress, qui ne conçoivent pas de travailler sans. Une part du besoin vient du fait que WordPress impose souvent de réutiliser les mêmes éléments à plusieurs endroits. Dans Osuny, beaucoup de choses fonctionnent par référence. Ainsi, quand on utilise un bloc “Actualités” pour lister des actus, on réutilise automatiquement les images à la une et les résumés de chaque actu. Il suffit de les modifier à la source pour que toutes les occurrences se mettent à jour automatiquement. 


Néanmoins, il y a deux cas qui rendent la bibliothèque de médias pertinente, et qui nous amène à envisager son développement. Le premier cas, c’est l’intégration d’une photothèque spécifique, avec des images dédiées. Osuny intègre de série les photothèques Unsplash et Pexels, avec une gestion des droits conforme. La possibilité d’une photothèque dédiée permet d’ajouter un ensemble d’images à la direction artistique cohérente, avec les bons lieux, les bons sujets, les bonnes personnes. L’autre cas pertinent, c’est la gestion des droits. Le fait de développer la bibliothèque de médias comme nous vous le proposons permettra dans le futur de développer une gestion fine des durées des droits négociés avec les photographes, de prévenir des fins de droits qui approchent et de mettre automatiquement hors ligne les photos dont les droits ont expiré. 


## Usages

### Dans WordPress

La bibliothèque de média est une fonctionnalité native de WordPress, ce qui donne l'impression aux personnes habituées à ce CMS que c'est indispensable. En réalité, la pertinence dans le cadre de WordPress vient beaucoup de la nécessité d'utiliser plusieurs fois les mêmes médias, notamment quand on veut construire du contenu avec des éditeurs de blocs. Ainsi, on peut être amené à utiliser un visuel deux fois : une fois pour illustrer une page, et une seconde fois pour mentionner la page avec une illustration et un lien au sein d'une autre page. Dans Osuny, les liens entre objets se font par l'utilisation de blocs de liens. Ainsi, si l'on veut présenter une page dans une autre, on utilise un bloc de Pages. On n'a pas besoin de renvoyer l'image, le lien se fait automatiquement. Et si l'on modifie l'image d'illustration de la page, tous les blocs mentionnant cette page se mettent à jour automatiquement. 

Néanmoins, il y a des usages pertinents qui méritent une explication.

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

### Implémentation

L'implémentation de la media library s'appuie de façon aussi légère que possible sur [ActiveStorage](https://guides.rubyonrails.org/active_storage_overview.html), le système natif de gestion des fichiers de Ruby on Rails. 

ActiveStorage s'appuie sur 3 tables.

```ruby
  create_table "active_storage_attachments", id: :uuid, default: -> { "public.gen_random_uuid()" }, force: :cascade do |t|
    t.string "name", null: false
    t.string "record_type", null: false
    t.uuid "record_id", null: false
    t.uuid "blob_id", null: false
    t.datetime "created_at", precision: nil, null: false
    t.index ["blob_id"], name: "index_active_storage_attachments_on_blob_id"
    t.index ["record_type", "record_id", "name", "blob_id"], name: "index_active_storage_attachments_uniqueness", unique: true
  end

  create_table "active_storage_blobs", id: :uuid, default: -> { "public.gen_random_uuid()" }, force: :cascade do |t|
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

  create_table "active_storage_variant_records", id: :uuid, default: -> { "public.gen_random_uuid()" }, force: :cascade do |t|
    t.uuid "blob_id", null: false
    t.string "variation_digest", null: false
    t.index ["blob_id", "variation_digest"], name: "index_active_storage_variant_records_uniqueness", unique: true
  end
```

`ActiveStorage::Blob` représente le fichier lui-même. On relie le Blob à un objet par le biais d'un `ActiveStorage::Attachment`, avec un attribut polymorphe `record`. Dans le contexte d'Osuny, les blobs sont liés à une université, afin de gérer le multi-tenant. La question des variantes n'a pas d'importance pour la médiathèque.

Malheureusement, ActiveStorage ne maintient pas l'unicité des Blobs : le même fichier, envoyé 2 fois, générera 2 blobs. 
Par ailleurs, la suppression d'un attachment provoque la suppression de son blob.

https://discuss.rubyonrails.org/t/activestorage-same-file-attached-multiple-times-single-blob-or-multiple-blobs/73028/3

Soit on compose avec ce fonctionnement, au prix de fichiers multiples, soit on hack ActiveStorage pour arriver à une unicité des Blobs.

```ruby
  # The medias themselves
  create_table "library_medias", id: :uuid, default: -> { "public.gen_random_uuid()" }, force: :cascade do |t|
    t.string "name", null: false
    # origin
    # 0   file uploaded through content (default)
    # 1   file uploaded through media library
    # 11  Unsplash
    # 12  Pexels
    t.integer "origin", null: false
    t.uuid "library_collection_id"
    t.datetime "created_at", precision: nil, null: false
    t.uuid "university_id"
  end

  # The collections
  create_table "library_collections", id: :uuid, default: -> { "public.gen_random_uuid()" }, force: :cascade do |t|
    t.string "name", null: false
    t.datetime "created_at", precision: nil, null: false
    t.uuid "university_id"
  end
```