---
title: "Mettre à jour le thème d'un site, V1 vers V2"
description: >
  Liste des étapes pour passer les sites en theme version 2.
---

Pour effectuer la migration : 

1. créer une branche “upgrade/theme” 
2. Si du js est ajouté au site, modifier les appels JS : 

```html
import './vendors/bootstrap';
import './vendors/lightbox';
import './vendors/carousel';
import './theme/body';
import './theme/cookie-banner';
```

3. Réécrire l’appel sass du thème

```html
@import "_theme/hugo-osuny"
```

4. Renommer les fichiers sass et le replacer dans _theme ou à la racine (si custom)
5. Ajouter le déploiement de la branch sur netlify : 

![Netlify Branch](/static/images/v1_to_v2-netlify-branches.png)

6. Ajouter la configuration de staging avec la bonne url dans . Exemple : 

```html
# Dans /config/staging/config.yaml
baseURL: https://upgrade-theme--awam-academy.netlify.app/
```

7. Modifier le fichier de configuration netlify pour ajouter le déploiement en environnement staging

```html
# Ajouter dans /netlify.toml
[context.branch-deploy.environment]
  HUGO_ENV = "staging"
```

8. Bien vérifier le fonctionnement de toutes les pages type en desktop et mobile à minima

9. Faire une PR
