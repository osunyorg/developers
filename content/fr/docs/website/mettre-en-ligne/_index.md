---
title: Mettre le site en ligne
weight: 4
description: >
  Comment publier le site sur le Web ?
---

Les sites produits avec Osuny utilisent Hugo, un générateur de site statique.
Ils doivent être précompilés, c'est à dire transformés en fichiers HTML, CSS et JS, afin d'être mis en ligne.
Une fois ces fichiers générés, il faut les envoyer sur le serveur ou dans le système qui les héberge.

## Préférer le SSH

La mise à jour des fichiers peut se faire en SSH ou en SFTP.
Il faut systématiquement privilégier le SSH, qui présente de meilleures performances.
Lors d'un test nous avons constaté que le même site est mis à jour en 20 secondes via SSH et en 3 minutes via SFTP.

## Publier selon l'hébergeur

La méthode de déploiement va différer selon l'hébergeur choisi. Vous avez des exemples pour les hébergeurs suivants :
- [Infomaniak](/docs/website/mettre-en-ligne/hebergeurs/infomaniak/)
- [Netlify](/docs/website/mettre-en-ligne/hebergeurs/netlify/)
- [OVH](/docs/website/mettre-en-ligne/hebergeurs/ovh/)

## Configuration serveur web

Il est possible que vous ayez besoin de configurer votre serveur web pour gérer certains aspects techniques. Par exemple :
- La redirection vers HTTPS
- La redirection d'un domaine racine vers le www (mon-domaine.fr => www.mon-domaine.fr) ou inversement
- La gestion des erreurs 404

Une explication détaillée est disponible selon votre serveur web :
- [Apache](/docs/website/mettre-en-ligne/serveurs-web/apache/) (dans la plupart des cas)
- [Nginx](/docs/website/mettre-en-ligne/serveurs-web/nginx/)
