---
title: Mettre le site en ligne
weight: 5
---

Les sites produits avec Osuny utilisent Hugo, un générateur de site statique.
Ils doivent être précompilés, c'est à dire transformés en fichiers HTML, CSS et JS, afin d'être mis en ligne. 
Une fois ces fichiers générés, il faut les envoyer sur le serveur ou dans le système qui les héberge.

## Avec Netlify

En connectant Netlify au référentiel Github, tout est pris en charge automatiquement.

[Suivre la documentation officielle](https://docs.netlify.com/welcome/add-new-site/#import-from-an-existing-repository)

## Avec Infomaniak

Pour publier chez Infomaniak, il faut utiliser une action Github, c'est à dire une tâche qui va s'exécuter à chaque modification du site.

### Trouver les informations de connexion
Les valeurs à remplir sont disponibles dans l'interface Infomaniak (référence/screenshot nécessaire).

### Intégrer ces informations dans Github
Dans Github, cliquer sur Settings, Secrets, Actions, et définir les variables suivantes :
``` yaml
SSH_HOST
SSH_PORT
SSH_PRIVATE_KEY
SSH_USER
SSH_WORKDIR
```

### Créer l'action automatisée
Créer le fichier `.github/workflows/deploy.yml`
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