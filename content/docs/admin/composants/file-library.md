---
title: Fichiers
---

## Introduction

https://roadmap.osuny.org/fonctionnalites/2026-espace-de-gestion-des-fichiers/

Le gestionnaire de fichiers a pour but de faciliter la gestion de l'ensemble des fichiers envoyés dans une instance.
La plupart des fichiers viennent des blocs, mais il y a aussi les fichiers liés directement aux formations.

L'intérêt est multiple : 
- éviter les doublons
- permettre la mise à jour centralisée
- fournir une base propre de fichiers aux équipes qui les utilisent
- faciliter la recherche dans cette base avec des critères multiples

## Principes

### Glossaire

| Nom | Définition |
|--|--|
| fichier physique | l'ensemble de données binaires que l'on upload ou download |
| blob | l'enregistrement dans la table `active_storage_blobs` |
| fichier logique | l'enregistrement dans la table `communication_files` |
| localisation | l'enregistrement dans la table `communication_file_localizations` |
| contextes | les cas d'usage d'une localisation |

### Ergonomie

L'envoi d'un fichier physique à la place d'un autre dans un bloc pose un problème ergonomique.

Il y a 2 possibilités : 
- soit la personne veut mettre à jour le fichier physique précédent, donc le remplacer
- soit la personne envoie un nouveau fichier, qui n'a pas de rapport avec le précédent

Pour résoudre simplement les choses, on ne peut mettre à jour que via la bibliothèque de fichiers.
Dans les blocs, tout ce qu'on envoie est considéré comme nouveau.

## Via les blocs

### Direct upload

Ce cas se passe lors de l'envoi via un bloc Fichiers.

```mermaid
graph TD;
  Envoi-->CreationBlob-->ChecksumCalculation-->Checksum
  Checksum-->ChecksumOui-->EnvoiInfos
  Checksum-->ChecksumNon-->ChecksumOtherLanguage
  ChecksumOtherLanguage-->ChecksumOtherLanguageOui-->CreationLoca-->EnvoiInfos
  ChecksumOtherLanguage-->ChecksumOtherLanguageNon-->CreationFichier-->EnvoiInfos

  Envoi["Envoi du fichier physique"]
  CreationBlob["Création du blob"]
  ChecksumCalculation["Calcul du checksum"]
  Checksum{"Le checksum existe-t-il dans cette langue ?"}
  ChecksumOui["Si le checksum existe"]
  ChecksumNon["Si le checksum n'existe pas"]
  ChecksumOtherLanguage{"Le checksum existe-t-il dans une autre langue ?"}
  ChecksumOtherLanguageOui["Si le checksum existe"]
  ChecksumOtherLanguageNon["Si le checksum n'existe pas"]
  CreationLoca["Création de la localisation et rattachement au fichier logique existant"]
  CreationFichier["Création du fichier logique et de sa localisation"]
  EnvoiInfos["Envoi de l'identifiant du fichier logique et du nom du fichier physique"]
```
À cette étape : 
1. le fichier physique existe sur Scaleway
2. le fichier logique existe dans la base de données avec sa localisation
3. mais le bloc n'a pas été enregistré

### Enregistrement d'un `about`

Lors de l'enregistrement du bloc (ou de la formation), on entre dans un autre flux.

```mermaid
graph TD;
  Enregistrement-->Fichiers-->Fichier-->Localisation;
  Localisation-->LocaNecessaire-->CreationLoca-->CreationContexte;
  Localisation-->LocaExiste;
  LocaExiste-->Contexte
  Contexte-->ContexteNecessaire-->CreationContexte;
  Contexte-->ContexteExiste-->Rien-->TraitementFini;
  CreationContexte-->TraitementFini-->ListeContextesObsoletes-->SuppressionContextesObsoletes;

  Enregistrement["Enregistrement de l'objet about (block ou program_localization)"]
  Fichiers["On liste les fichiers logiques dont les identifiants sont envoyés"]
  Fichier(["Pour chaque fichier, on récupère sa localisation"])
  Localisation{"La localisation du fichier existe-t-elle ?"}
  LocaExiste["La localisation existe"]
  LocaNecessaire["La localisation n'existe pas encore (cf traduction ci-dessous)"]
  CreationLoca["Création de la localisation en dupliquant la localisation originale du fichier"]
  Contexte{"Le contexte existe-t-il ?"}
  ContexteNecessaire["Le contexte n'existe pas encore"]
  ContexteExiste["Le contexte existe déjà"]
  CreationContexte["Création du contexte"]
  Rien["Aucune action"]
  TraitementFini(["Une fois le traitement de tous les fichiers terminé"])
  ListeContextesObsoletes["On liste les contextes de l'objet mentionnant d'autres fichiers"]
  SuppressionContextesObsoletes["Suppression des contextes obsolètes"]
```

### Traduction

Cela se passe parce que l'on traduit l'objet lié (la page dans laquelle est le bloc, par exemple).

Il y a 2 options pour le traitement, mais l'une des 2 est mauvaise.
Les 2 sont documentées ci-dessous.

Mauvaise option : absence de localisation

```mermaid
graph TD;
  Traduction-->Loca
  Loca-->LocaExiste-->EnregistrementAvec
  Loca-->LocaManquante-->GarderOuSupprimer
  GarderOuSupprimer-->Supprimer-->MauvaisChoix
  GarderOuSupprimer-->Garder-->EnregistrementSansLoca-->UtilisationSansLoca
  UtilisationSansLoca-->Rien-->MauvaisChoix
  UtilisationSansLoca-->Substitution-->MauvaisChoix
  
  Traduction["Traduction du bloc"]
  Loca{"La localisation existe-t-elle ?"}
  LocaExiste["La localisation existe pour la nouvelle langue"]
  LocaManquante["La localisation n'existe pour la nouvelle langue"]
  GarderOuSupprimer{"Faut-il garder le lien ou le supprimer ?"}
  Supprimer["Supprimer l'identifiant du fichier, tout a disparu"]
  EnregistrementAvec["Enregistrement normal"]
  EnregistrementSansLoca["Enregistrement de l'identifiant du fichier qui n'a pas sa localisation"]
  UtilisationSansLoca{"Que faire en affichage dans l'admin ou en export statique ?"}
  MauvaisChoix["Mauvais choix architectural"]
  Rien["Ne rien afficher ? Cela revient à supprimer, sauf qu'on peut éventuellement crééer la localisation via le gestionnaire de fichiers."]
  Substitution["Utiliser la localisationn originale ? C'est exactement comme l'option duplication, mais en plus compliqué."]
```

{{< callout type="warning" >}}
Cette option créée des complications architecturales et ergonomiques.
{{< /callout >}}

Bonne option : duplication du fichier original

```mermaid
graph TD;
  Traduction-->Loca
  Loca-->LocaExiste-->Enregistrement
  Loca-->LocaManquante-->CreationLoca-->Enregistrement

  Traduction["Traduction du bloc"]
  Loca{"La localisation existe-t-elle ?"}
  LocaExiste["La localisation existe pour la nouvelle langue"]
  LocaManquante["La localisation n'existe pas pour la nouvelle langue"]
  CreationLoca["Création de la localisation avec le blob de la localisation originale"]
  Enregistrement["Enregistrement avec l'identifiant du fichier (donc non linguistique)"]
```

### Suppression

Lors de la suppression d'un bloc, d'une formation ou autre, il faut détruire les contextes dont l'objet est l'about.
Comme c'est une propriété polymorphe, il faut passer par un before_destroy.

## Via la bibliothèque de fichiers

### Création

```mermaid
graph TD;
  SoumissonFormulaire-->AnalyseFichierPhysique-->CheckIntegrite
  CheckIntegrite-->IntegreNon-->Erreur
  CheckIntegrite-->IntegreOui-->CheckUnicite
  CheckUnicite-->UniqueNon-->Erreur
  CheckUnicite-->UniqueOui-->UploadFichierPhysique-->CreationFichierLogique

  SoumissonFormulaire["On soumet le formulaire de création de fichier logique"]
  AnalyseFichierPhysique["Analyse du fichier physique"]
  CheckIntegrite{"Le fichier physique est-il intégre ? (Est bien un fichier, taille autorisée)"}
  IntegreNon["Le fichier n'est pas intègre"]
  Erreur["Renvoi d'une erreur"]
  IntegreOui["Le fichier est intègre"]
  CheckUnicite{"Le fichier physique est-il unique ? (à partir du checksum)"}
  UniqueNon["Le fichier physique existe déjà"]
  UniqueOui["Le fichier physique n'existe pas encore"]
  UploadFichierPhysique["Upload du fichier physique sur Scaleway"]
  CreationFichierLogique["Création du fichier logique et de sa localisation"]
```

À cette étape : 
1. le fichier physique existe sur Scaleway
2. le fichier logique existe dans la base de données avec sa localisation

### Modification

Il s'agit là de mettre à jour le fichier physique, ou les propriétés de la localisation (nom, catégories, description interne, image).
Tout le début est identique à l'enregistrement, on reprend à la dernière étape.

```mermaid
graph TD;
  Enregistrement-->Contextes-->UpdateName-->UpdateImage-->RegenerateGitFiles

  Enregistrement["Enregistrement du fichier logique et de sa localisation"]
  Contextes([Pour chaque contexte])
  UpdateName["Mise à jour du nom de fichier s'il était identique à la valeur avant changement"]
  UpdateImage["Mise à jour de l'image si elle était identique à la valeur avant changement"]
  RegenerateGitFiles["Régénération du GitFile"]
```

### Suppression

On ne peut supprimer que les fichiers sans contexte.

### Fusion

Il y aura, tôt ou tard, le besoin de fusionner des fichiers qui sont en fait des versions locales l'un de l'autre.

