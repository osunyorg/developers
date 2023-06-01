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

## Gérer les erreurs 404

Si vous déployez votre site vers un serveur web que vous gérez vous-mêmes, pensez à le configurer pour bien gérer les erreurs 404.

### Apache

Dans la configuration Apache de votre site :

```
<VirtualHost *:443>
  ServerName www.example.fr
  DocumentRoot /var/www/chemin/vers/votre/site
  DirectoryIndex index.html
  ErrorDocument 404 /404.html
</VirtualHost>
```

### Nginx

Dans la configuration Nginx de votre site :

```
server {
  listen 443 ssl http2;
  server_name www.exemple.fr;
  root /var/www/chemin/vers/votre/site;
  index index.html;
  error_page 404 /404.html;
}
```
