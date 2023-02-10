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

## Avec Netlify

En connectant Netlify au référentiel Github, tout est pris en charge automatiquement.

[Suivre la documentation officielle](https://docs.netlify.com/welcome/add-new-site/#import-from-an-existing-repository)

Pour déployer le site avec Netlify, penser à ajouter la deploy key. 
TODO comment faire sans deploy key? 

## Avec Infomaniak en SSH

Pour publier chez Infomaniak, il faut utiliser une action Github, c'est à dire une tâche qui va s'exécuter à chaque modification du site.

Trouver les informations de connexion dans l'interface Infomaniak (référence/screenshot nécessaire).

Intégrer ces informations dans Github (cliquer sur Settings, Secrets, Actions), et définir les variables suivantes :
``` yaml
SSH_HOST
SSH_PORT
SSH_PRIVATE_KEY
SSH_USER
SSH_WORKDIR
```

Créer l'action automatisée dans le fichier `.github/workflows/deploy.yml`
``` yaml
name: SSH Deployment

on:
  push:
    branches:
      - main  # Set a branch to deploy
jobs:
  deploy:
    runs-on: ubuntu-20.04
    concurrency:
      group: ${{ github.workflow }}
      cancel-in-progress: true
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 1    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: '16'
          cache: 'yarn'

      - name: Install JS dependencies
        run: yarn install --frozen-lockfile

      - name: Build
        run: hugo -e production --minify

      - name: Install SSH Key
        uses: shimataro/ssh-key-action@v2
        with:
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          known_hosts: 'just-a-placeholder-so-we-dont-get-errors'

      - name: Adding Known Hosts
        run: ssh-keyscan -p ${{ secrets.SSH_PORT }} -H ${{ secrets.SSH_HOST }} >> ~/.ssh/known_hosts

      - name: Deploy with rsync
        run: rsync -avz --delete -e "ssh -p ${{ secrets.SSH_PORT }}" ./public/ ${{ secrets.SSH_USER }}@${{ secrets.SSH_HOST }}:${{ secrets.SSH_WORKDIR }}/
```

## Avec OVH en SFTP

Trouver les informations de connexion dans l'interface OVH

![Interface OVH](/images/ovh.png)

Intégrer ces informations dans Github (cliquer sur Settings, Secrets, Actions), et définir les variables suivantes.
Les valeurs entre crochets sont indicatives, à remplacer par les vraies valeurs.
```yaml
FTP_HOSTNAME: ftp.[adresse du cluster].hosting.ovh.net
FTP_USERNAME: [nom de l'utilisateur]
FTP_PASSWORD: [mot de passe]
FTP_PORT: 22
FTP_LOCAL_DIR: ./public/*
FTP_SERVER_DIR: /home/[nom du répertoire]/www
```
Créer l'action automatisée dans le fichier `.github/workflows/deploy-sftp.yml`
```yaml
name: FTP deployment

on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-20.04
    concurrency:
      group: ${{ github.workflow }}
      cancel-in-progress: true
    steps:
      - uses: actions/checkout@v3
        with:
          submodules: true
          fetch-depth: 1

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: '16'
          cache: 'yarn'

      - name: Install JS dependencies
        run: yarn install --frozen-lockfile

      - name: Build
        run: hugo -e production --minify

      - name: Sync files
        uses: wlixcc/SFTP-Deploy-Action@v1.2.4
        with:
          server: ${{ secrets.FTP_HOSTNAME }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          port: ${{ secrets.FTP_PORT }}
          local_path: ${{ secrets.FTP_LOCAL_DIR }}
          remote_path: ${{ secrets.FTP_SERVER_DIR }}
          sftp_only: true
```

## Gérer la préproduction

Il peut être nécessaire ou utile d'avoir une version de staging du site. Pour préparer cette version il faut set la variable d’environnement **HUGO_ENV** à **staging** (ou le nom de la configuration voulue dans /config de votre site) lors d'un déploiement *branch-deploy* :

Ajouter dans le fichier netlify.toml :

```toml
[context.branch-deploy.environment]
  HUGO_ENV = "staging"
```

Il faut également modifier le fichier de config staging (config/staging/config.yaml) pour mettre l'url netlify de la branche en question :

```yaml
baseURL: https://staging--osuny-www.netlify.app/
```

![Netlify Branch](/static/images/v1_to_v2-netlify-branches.png)

Cette configuration permet de déployer une seule autre branche, mais un setup libre devrait permettre plusieurs version / branches.

Par exemple, dans config/staging/config.yaml :

```yaml
baseURL: /
```

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
