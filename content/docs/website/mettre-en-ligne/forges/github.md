---
title: GitHub
weight: 49
---

Que ce soit pour publier en SSH ou en FTP, il faut utiliser une action GitHub, c'est à dire une tâche qui va s'exécuter à chaque modification du site.

## SSH

Assurez-vous d'avoir toutes l'informations nécessaires :
- `SSH_HOST` : `[serveur hôte ci-dessus]`
- `SSH_PORT` : `[port SSH (par défaut 22)]`
- `SSH_PRIVATE_KEY` : `[la clé privée SSH]`
- `SSH_USER` : `[nom de l'utilisateur ci-dessus]`
- `SSH_WORKDIR` : `[dossier vers le site (exemple : /var/www)]`

### Configuration du déploiement

Aller sur GitHub, dans "Settings", "Secrets and variables", "Actions", puis dans l'onglet "Secrets", définissez les *repository secrets* ci-dessus.

Créer l'action automatisée dans le fichier `.github/workflows/deploy.yml`

```yaml
name: SSH Deployment

on:
  push:
    branches:
      - main  # Set a branch to deploy
jobs:
  deploy:
    runs-on: ubuntu-latest
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
          hugo-version: '0.159.2'
          extended: true

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 'lts/*'
          cache: 'yarn'

      - name: Install JS dependencies
        run: yarn install --frozen-lockfile

      - name: Build
        run: yarn osuny build

      - name: Install SSH Key
        uses: shimataro/ssh-key-action@v2
        with:
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          known_hosts: 'just-a-placeholder-so-we-dont-get-errors'

      - name: Adding Known Hosts
        run: ssh-keyscan -p ${{ secrets.SSH_PORT }} -H ${{ secrets.SSH_HOST }} >> ~/.ssh/known_hosts

      - name: Deploy with rsync
        run: rsync -avz --delete -e "ssh -p ${{ secrets.SSH_PORT }}" ./public/ ${{ secrets.SSH_USER }}@${{ secrets.SSH_HOST }}:${{ secrets.SSH_WORKDIR }}/

      - name: Notification Slack en cas d'échecs
        uses: ravsamhq/notify-slack-action@2.3.0
        if: always()
        with:
          status: ${{ job.status }}
          notify_when: "failure"
          notification_title: ""
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

## FTP

Assurez-vous d'avoir toutes l'informations nécessaires :
- `FTP_HOSTNAME` : `[serveur hôte ci-dessus]`
- `FTP_USERNAME` : `[nom de l'utilisateur ci-dessus]`
- `FTP_PASSWORD` : `[mot de passe ci-dessus]`
- `FTP_PORT` : `21`
- `FTP_LOCAL_DIR` : `./public/`
- `FTP_SERVER_DIR` : `[dossier vers le site]/`

*NOTE : Ne pas oublier le `/` à la fin du `FTP_SERVER_DIR`. Exemple : `/var/www/`*

## Configuration du déploiement

Aller sur GitHub, dans "Settings", "Secrets and variables", "Actions", puis dans l'onglet "Secrets", définissez les *repository secrets* ci-dessus.

Créer l'action automatisée dans le fichier `.github/workflows/deploy.yml`

```yaml
name: FTP deployment

on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest
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
          hugo-version: '0.159.2'
          extended: true

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 'lts/*'
          cache: 'yarn'

      - name: Install JS dependencies
        run: yarn install --frozen-lockfile

      - name: Build
        run: yarn osuny build

      - name: SFTP Deploy
        uses: SamKirkland/FTP-Deploy-Action@v4.3.4
        with:
          server: ${{ secrets.FTP_HOSTNAME }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          local-dir: ${{ secrets.FTP_LOCAL_DIR }}
          server-dir: ${{ secrets.FTP_SERVER_DIR }}

      - name: Notification Slack en cas d'échec
        uses: ravsamhq/notify-slack-action@2.3.0
        if: always()
        with:
          status: ${{ job.status }}
          notify_when: "failure"
          notification_title: ""
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```
