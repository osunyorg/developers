---
title: Création du site dans le CMS
weight: 100
description: >-
  Créer le site sur Osuny
---

## Étape 1

- Se connecter à l'instance / université Osuny
- Aller dans Site web
- Créer
- Remplir les champs du site, puis enregistrer

## Étape 2

- https://github.com/noesya/osuny-hugo-template-aaa
- Cliquer sur "use this template" / Create a New Repository
- nommer le repo [IDENTIFIANT_DE_L_UNIVERSITE] - [NOM_DU_SITE]

## Étape 3

- De retour dans la configuration du site sur Osuny
- Copier le path du repository GitHub (ex : noesya/bordeauxmontaigne-iut)
- Ensuite il faut récupérer le token GitHub (ou GitLab) (méthode de récupération du token à documenter)

## Étape 4

- Mise en ligne avec Netlify
- Se connecter au compte netlify
- Créer un nouveau site (Add new site / Import an existing project)
- Choisir le repository du site
- Déployer (en l'état, le premier deploy crash)
- Aller dans "deploys / deploy settings" puis "build & deploys" : Générer une deploy key, puis copier la clé
- Aller dans le repo du thème Hugo utilisé (en passant par le dossier `themes` du repo du site)
- Aller dans la partie Settings
- Aller dans la partie Deploy keys
- Ajouter une clé avec le nom du site et coller la clé copiée depuis Netlify. Ne pas cocher "Allow write access".
- À documenter : explication sur les token GitHub / Netlify

## Étape 5

- Une fois le site déployer, copier / coller l'url du site public dans la configuration du site dans l'admin Osuny

