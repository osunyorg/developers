---
title: Composants Vue
---

## Introduction

Ce document fournit une documentation complète des composants Vue.js utilisés dans le projet Osuny. Le projet utilise Vue 3 pour créer des interfaces utilisateur interactives et réactives dans différentes parties de l'application administrative.

## Structure des composants Vue

Les composants Vue sont organisés en plusieurs applications principales :

1. **Composants Généraux** - Composants réutilisables
2. **Media Picker** - Gestion des médias et des images
3. **Blocks Editor** - Éditeur de blocs de contenu
4. **Time Slots** - Gestion des créneaux horaires
5. **SSO Mapping** - Mappage des champs SSO

## Liste complète des composants

### 1. Composants généraux

`/app/javascript/apps/components/`

#### CropperModal.vue
- **Description** : Modal pour recadrer les images avec vue-advanced-cropper
- **Fonctionnalités** :
  - Recadrage d'images avec rotation
  - Interface modale avec aperçu
  - Envoi des données de recadrage au serveur
  - Gestion des états de chargement
- **Dépendances** : `vue-advanced-cropper`
- **Événements** :
  - `@cropped` - Émis lorsque l'image est recadrée

#### Summernote.vue
- **Description** : Éditeur de texte riche basé sur Summernote
- **Fonctionnalités** :
  - Édition de texte riche WYSIWYG
  - Support multilingue
  - Intégration avec le système de traduction
- **Utilisation** : Utilisé pour les champs de texte riche comme les crédits d'image

#### Changes.vue
- **Description** : Composant pour suivre et afficher les changements
- **Fonctionnalités** :
  - Suivi des modifications
  - Affichage des différences
  - Gestion des versions

### 2. Media Picker 

`/app/javascript/apps/media-picker/`

#### MediaPickerApp.vue (Application principale)
- **Description** : Application principale pour la sélection et la gestion des médias
- **Fonctionnalités** :
  - Sélection d'images depuis différentes sources
  - Gestion des métadonnées des images (alt, crédit)
  - Intégration avec Summernote pour les crédits
  - Suivi des changements
- **Composants enfants** :
  - ImageUploader
  - Cloud
  - Medias
  - Summernote
  - Changes

#### ImageUploader.vue
- **Description** : Composant pour téléverser des images
- **Fonctionnalités** :
  - Téléchargement de fichiers
  - Prévisualisation des images
  - Gestion des erreurs de téléchargement
- **Événements** :
  - `@uploaded` - Émis lorsque le téléchargement est terminé

#### Cloud.vue
- **Description** : Intégration avec des services cloud d'images
- **Fonctionnalités** :
  - Connexion à Unsplash
  - Connexion à Pexels
  - Recherche et sélection d'images
  - Gestion des crédits automatiques
- **Événements** :
  - `@unsplashSelected` - Image sélectionnée depuis Unsplash
  - `@pexelsSelected` - Image sélectionnée depuis Pexels

#### Medias.vue
- **Description** : Bibliothèque de médias locale
- **Fonctionnalités** :
  - Affichage des médias disponibles
  - Filtrage et recherche
  - Sélection de médias existants
- **Événements** :
  - `@mediaSelected` - Médias sélectionné depuis la bibliothèque

#### Taxonomies.vue
- **Description** : Gestion des taxonomies pour les médias
- **Fonctionnalités** :
  - Organisation des médias par catégories
  - Filtrage par taxonomie
  - Interface de sélection

#### Categories.vue
- **Description** : Gestion des catégories pour les médias
- **Fonctionnalités** :
  - Organisation hiérarchique
  - Sélection multiple
  - Interface utilisateur intuitive

### 3. Blocks Editor 

`/app/javascript/apps/blocks-editor/`

#### BlocksEditorApp.vue (Application principale)
- **Description** : Éditeur de blocs de contenu pour la création de pages
- **Fonctionnalités** :
  - Gestion des blocs de contenu
  - Réorganisation par glisser-déposer
  - Mode plan pour la visualisation
  - Édition en contexte
  - Gestion des versions
- **Composants enfants** :
  - Blocks
  - Editor
  - OffcanvasShell
  - TemplatePicker

#### Blocks.vue
- **Description** : Liste et gestion des blocs
- **Fonctionnalités** :
  - Affichage de la liste des blocs
  - Réorganisation des blocs
  - Actions sur les blocs (éditer, dupliquer, supprimer)
  - Gestion des états

#### Editor.vue
- **Description** : Éditeur de bloc individuel
- **Fonctionnalités** :
  - Édition des propriétés du bloc
  - Validation et sauvegarde
  - Interface modale
- **Événements** :
  - `@save` - Sauvegarde du bloc
  - `@close` - Fermeture de l'éditeur

#### OffcanvasShell.vue
- **Description** : Conteneur pour l'interface off-canvas
- **Fonctionnalités** :
  - Gestion de l'état (ouvert/fermé)
  - Animation et transitions
  - Accessibilité

#### TemplatePicker.vue
- **Description** : Sélecteur de templates pour nouveaux blocs
- **Fonctionnalités** :
  - Liste des templates disponibles
  - Prévisualisation
  - Création de nouveaux blocs
- **Événements** :
  - `@created` - Nouveau bloc créé

#### Inputs (composants d'entrée spécialisés)

##### MultiImageInput.vue
- **Description** : Champ de saisie pour plusieurs images
- **Fonctionnalités** :
  - Sélection multiple
  - Prévisualisation
  - Réorganisation

##### CodeInput.vue
- **Description** : Éditeur de code avec coloration syntaxique
- **Fonctionnalités** :
  - Support de plusieurs langages
  - Numérotation des lignes
  - Thèmes personnalisables

##### UploadInput.vue
- **Description** : Champ de téléchargement de fichiers
- **Fonctionnalités** :
  - Glisser-déposer
  - Validation des types de fichiers
  - Progression du téléchargement

##### RichTextInput.vue
- **Description** : Éditeur de texte riche intégré
- **Fonctionnalités** :
  - Outils de mise en forme
  - Insertion de médias
  - Gestion des liens

### 4. Time Slots 

`/app/javascript/apps/time-slots/`

#### TimeSlotsApp.vue (Application principale)
- **Description** : Gestion des créneaux horaires
- **Fonctionnalités** :
  - Création et modification de créneaux
  - Gestion des disponibilités
  - Interface calendaire

#### Duration.vue
- **Description** : Composant pour la gestion de la durée
- **Fonctionnalités** :
  - Sélection de la durée
  - Conversion entre formats
  - Validation

### 5. SSO Mapping 

`/app/javascript/apps/sso-mapping/`

#### SsoMappingApp.vue (Application principale)
- **Description** : Application pour mapper les champs SSO (Single Sign-On) aux champs internes
- **Fonctionnalités** :
  - Mappage des champs SSO aux champs internes de l'application
  - Interface glisser-déposer pour réorganiser les champs
  - Gestion des rôles et permissions
  - Configuration flexible des correspondances
- **Composants enfants** :
  - VueDraggableNext (pour le glisser-déposer)
- **Fonctionnalités détaillées** :
  - Ajout dynamique de nouveaux champs de mappage
  - Sélection des clés SSO et des clés internes
  - Gestion des rôles associés à chaque mappage
  - Interface utilisateur intuitive avec réorganisation par glisser-déposer
  - Activation automatique des champs de sélection
- **Données** :
  - `fields` : Liste des champs de mappage configurés
  - `keys` : Liste des clés disponibles pour le mappage
- **Méthodes principales** :
  - `addField()` : Ajoute un nouveau champ de mappage
  - `enableSelects()` : Active les champs de sélection après manipulation du DOM

## Architecture technique

### Structure des fichiers
```
app/javascript/apps/
├── components/             # Composants réutilisables
├── media-picker/           # Application de sélection de médias
│   ├── components/         # Composants spécifiques
│   └── MediaPickerApp.vue  # Point d'entrée
├── blocks-editor/          # Éditeur de blocs
│   ├── components/         # Composants spécifiques
│   │   ├── inputs/         # Composants d'entrée
│   └── BlocksEditorApp.vue # Point d'entrée
├── time-slots/             # Gestion des créneaux
│   ├── components/         # Composants spécifiques
│   └── TimeSlotsApp.vue    # Point d'entrée
└── sso-mapping/            # Mappage SSO
    └── SsoMappingApp.vue   # Point d'entrée
```


### Bonnes pratiques

1. **Internationalisation** : Tous les composants utilisent le système de traduction avec `$t()`
2. **Accessibilité** : Respect des standards ARIA et accessibilité
3. **Gestion d'état** : Utilisation de Vuex pour la gestion d'état globale
4. **Communication** : Événements personnalisés pour la communication entre composants
5. **Style** : Utilisation de classes CSS spécifiques avec préfixe `vue__`

### Intégration avec Rails

Les applications Vue sont intégrées dans l'application Rails :

1. **Points de montage** : Chaque application Vue a un point de montage HTML
2. **Données initiales** : Transmission des données via `data-*` attributes
3. **CSRF** : Gestion des tokens CSRF pour les requêtes AJAX
4. **Routes** : Utilisation des routes Rails pour les endpoints API

## Exemples d'utilisation

### Intégration de MediaPickerApp

```erb
<div id="media-picker-app"
     data-current='<%= @current.to_json %>'
     data-changes-endpoint="<%= changes_path %>">
</div>
````

```javascript
// Dans un fichier JavaScript de pack
import MediaPickerApp from '../apps/media-picker/MediaPickerApp.vue';

document.addEventListener('DOMContentLoaded', () => {
  const appElement = document.getElementById('media-picker-app');
  if (appElement) {
    const app = Vue.createApp(MediaPickerApp);
    app.mount(appElement);
  }
});
```

### Utilisation de CropperModal

```javascript
// Dans un composant parent
methods: {
  openCropper(blob) {
    this.$refs.cropperModal.launch(blob);
  },
  onCropped(blob) {
    // Gérer l'image recadrée
  }
}
```

```html
<CropperModal ref="cropperModal" @cropped="onCropped" />
```

## Dépendances externes

- `vue-advanced-cropper` - Pour le composant CropperModal
- `summernote` - Éditeur de texte riche
- `notyf` - Notifications utilisateur
- `bootstrap-icons` - Icônes utilisées dans l'interface

