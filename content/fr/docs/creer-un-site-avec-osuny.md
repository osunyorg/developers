---
title: "Créer un site avec Osuny"
linkTitle: "Créer un site avec Osuny"
weight: 100
description: >-
     Étape pour créer un site avec Osuny
---

# Étape 1

- Se connecter à l'instance / université Osuny
- Aller dans Site web
- Créer
- Remplir les champs du site, puis enregistrer

# Étape 2

- https://github.com/noesya/osuny-hugo-template-aaa
- Cliquer sur "use this template"
- nommer le repo [IDENTIFIANT_DE_L_UNIVERSITE] - [NOM_DU_SITE]

# Étape 3

- De retour dans la configuration du site sur Osuny
- Copier le path du repository GitHub (ex : noesya/bordeauxmontaigne-iut)
- Ensuite il faut récupérer le token GitHub (ou GitLab) (méthode de récupération du token à documenter)

# Étape 4

- Mise en ligne avec Netlify
- Se connecter au compte netlify
- Créer un nouveau site
- Choisir le repository du site
- Déployer (en l'état, le premier deploy crash)
- Aller dans "deploy settings" : Générer une deploy key, puis copier la clé
- Copier / coller la clé dans le repo theme osuny sur github 
- À documenter : explication sur les token GitHub / Netlify

# Étape 5

- Une fois le site déployer, copier / coller l'url du site public dans la configuration du site dans l'admin Osuny

