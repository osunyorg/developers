---
title: Nginx
weight: 2
description: >
  Configurer son serveur web Nginx
---

Dans le cas d'un serveur Nginx, la configuration ne peut pas être faite par un fichier dans le dossier du site. Il vous faudra un accès au fichier de configuration via SSH (souvent dans `/etc/nginx/nginx.conf` ou dans l'un des dossiers `conf.d` et `sites-enabled` présents dans `/etc/nginx`).

Pour que votre site soit accessible depuis votre domaine, ajouter le bloc suivants dans le fichier de configuration et modifier le `server_name` par le domaine de votre choix :

```
server {
  listen 443 ssl http2;
  server_name www.mon-domaine.fr;
  root /var/www/chemin/vers/votre/site;
  index index.html;
  error_page 404 /404.html;
}
```

## La gestion des erreurs 404

Pour afficher la page 404 générée par Hugo, ajouter la ligne `error_page` présente dans le bloc ci-dessus.

## Redirections

### Vers HTTPS

Pour rediriger vers HTTPS, ajouter ce bloc dans votre configuration

```
server {
    listen 80 default_server;
    server_name _;
    return 301 https://www.mon-domaine.fr$request_uri;
}
```

*NOTE : Pensez à modifier le domaine sur la dernière ligne.*

## D'un domaine racine vers le www

*Exemple : `mon-domaine.fr` => `www.mon-domaine.fr`*

Pour rediriger vers le www, copier-coller ces lignes dans votre configuration

```
server {
    listen 80;
    listen 443 ssl;
    server_name mon-domaine.fr;
    return 301 https://www.mon-domaine.fr$request_uri;
}
```

## D'un www vers le domaine racine

*Exemple : `www.mon-domaine.fr` => `mon-domaine.fr`*

Pour rediriger vers le domaine racine, copier-coller ces lignes dans votre configuration

```
server {
    listen 80;
    listen 443 ssl;
    server_name www.mon-domaine.fr;
    return 301 https://mon-domaine.fr$request_uri;
}
```
