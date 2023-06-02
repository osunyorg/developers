---
title: Apache
weight: 1
description: >
  Configurer son serveur web Apache
---

Dans le cas d'un serveur Apache, la configuration peut être faite par un fichier `.htaccess` à placer dans le dossier `static` de votre site. Il sera ensuite déployé à la racine en fichier caché.

Voici une liste de directives en fonction de vos besoins à ajouter dans votre `.htaccess`.

## La gestion des erreurs 404

Pour afficher la page 404 générée par Hugo, copier-coller cette ligne dans le fichier `.htaccess`

```
ErrorDocument 404 /404.html
```

## Redirections

Cette ligne est nécessaire dans le fichier `.htaccess` pour les redirections ci-dessous. Ajoutez-la au début du fichier.

```
RewriteEngine On
```

### Vers HTTPS

Pour rediriger vers HTTPS, copier-coller ces lignes dans le fichier `.htaccess`

```
RewriteCond %{HTTPS} off
RewriteRule (.*) https://%{HTTP_HOST}%{REQUEST_URI} [R,L]
```

**ATTENTION** : si vous avez une boucle de redirection (possible sur Infomaniak), rajoutez ceci sous la ligne `RewriteCond %{HTTPS} off` :

```
RewriteCond %{HTTP:X-Forwarded-Proto} !https
```

## D'un domaine racine vers le www

*Exemple : `mon-domaine.fr` => `www.mon-domaine.fr`*

Pour rediriger vers le www, copier-coller ces lignes dans le fichier `.htaccess`

```
RewriteCond %{HTTP_HOST} ^mon\-domaine\.fr [NC]
RewriteRule ^(.*)$ https://www.mon-domaine.fr/$1 [L,R=301]
```

**ATTENTION** : pensez à bien utiliser le caractère `\` avant les caractères spéciaux (`.`, `-`, ...) sur la ligne `RewriteCond`.

## D'un www vers le domaine racine

*Exemple : `www.mon-domaine.fr` => `mon-domaine.fr`*

Pour rediriger vers le domaine racine, copier-coller ces lignes dans le fichier `.htaccess`

```
RewriteCond %{HTTP_HOST} ^www\.mon\-domaine\.fr [NC]
RewriteRule ^(.*)$ https://mon-domaine.fr/$1 [L,R=301]
```

**ATTENTION** : pensez à bien utiliser le caractère `\` avant les caractères spéciaux (`.`, `-`, ...) sur la ligne `RewriteCond`.