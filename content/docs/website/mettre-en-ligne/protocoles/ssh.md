---
title: SSH
weight: 49
---

Pour publier sur un serveur en SSH, il faut utiliser une action GitHub, c'est à dire une tâche qui va s'exécuter à chaque modification du site.

Assurez-vous d'avoir toutes l'informations nécessaires :
- `SSH_HOST` : `[serveur hôte ci-dessus]`
- `SSH_PORT` : `[port SSH (par défaut 22)]`
- `SSH_PRIVATE_KEY` : `[la clé privée SSH]`
- `SSH_USER` : `[nom de l'utilisateur ci-dessus]`
- `SSH_WORKDIR` : `[dossier vers le site (exemple : /var/www)]`

## Configuration du déploiement

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
