---
title: Fichiers
---

https://roadmap.osuny.org/fonctionnalites/2026-espace-de-gestion-des-fichiers/

Le gestionnaire de fichiers a pour but de faciliter la gestion de l'ensemble des fichiers envoyés dans une instance.
La plupart des fichiers viennent des blocs, mais il y a aussi les fichiers liés directement aux formations.

L'intérêt est multiple : 
- éviter les doublons
- permettre la mise à jour centralisée
- fournir une base propre de fichiers aux équipes qui les utilisent

## Upload

```mermaid
graph TD;
  Envoi-->CreationBlob-->Checksum-->Fichier;
  Fichier-->FichierOui-->EnvoiInfos;
  Fichier-->FichierNon-->CreationFichier-->EnvoiInfos;

  Envoi["Envoi du fichier"];
  CreationBlob["Création du blob"];
  Checksum["Calcul du checksum"];
  Fichier["Le fichier existe-t-il ?"];
  FichierOui["Si le fichier existe déjà"];
  FichierNon["Si le fichier n'existe pas"];
  CreationFichier["Création du fichier"];
  EnvoiInfos["Envoi des informations sur le fichier"]
```

À cette étape, le fichier physique est sur Scaleway, le fichier est créé, mais le bloc n'a pas été enregistré.
Lors de l'enregistrement, on entre dans un autre flux.


```mermaid
graph TD;
  Enregistrement-->Contextes-->Contexte;
  Contexte-->ContexteNecessaire-->Rien-->FichiersSansContexte;
  Contexte-->ContexteObsolete-->SuppressionContexte-->FichiersSansContexte;
  FichiersSansContexte-->FichierSansContexte-->CreationContexte

  Enregistrement["Enregistrement de l'objet about (block ou formation_localization)"]
  Contextes["Récupération des contextes de l'objet"]
  Contexte["Pour chaque contexte"]
  SuppressionContexte["Suppression du contexte"]
  CreationContexte["Création du contexte"]
  Rien["Aucune action"]
  FichiersSansContexte["Liste des fichiers sans contexte"]
  FichierSansContexte["Pour chaque fichier sans contexte"]
```
