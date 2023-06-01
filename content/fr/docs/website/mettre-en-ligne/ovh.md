---
title: OVH
---

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
