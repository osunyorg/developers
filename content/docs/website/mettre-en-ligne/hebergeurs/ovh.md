---
title: OVH
---


## Registrar ou hébergeur ?

On peut soit utiliser OVH comme registrar, pour faire pointer son nom de domaine vers Deuxfleurs ou ailleurs, soit l'utiliser pour héberger.

## OVH comme registrar

Il faut faire un pointage CNAME 

```DNS
www IN CNAME production.osuny.site.
```

Comme OVH ne permet pas les ALIAS à l'apex, il faut faire une redirection. Malheureusement, OVH ne sait pas non plus faire des redirections en HTTPS à l'apex, donc il faut activer l'hébergement gratuit, et poser un fichier .htaccess avec un redirect

Ici, pour l'OPCD.

```htaccess
RedirectPermanent / https://www.opcd.co
```


## OVH comme hébergeur 

Trouver les informations de connexion dans l'interface OVH

![Interface OVH](/images/ovh.png)

A partir de l'hébergement Pro, on peut utiliser le SSH. Mais sinon, il faut passer par la méthode FTP.

### Méthode FTP

Aller sur GitHub, dans "Settings", "Secrets and variables", "Actions", puis dans l'onglet "Secrets", définissez les *repository secrets* suivants.

Les valeurs entre crochets sont indicatives, à remplacer par les vraies valeurs.

- `FTP_HOSTNAME` : `ftp.[adresse du cluster].hosting.ovh.net`
- `FTP_USERNAME` : `[nom de l'utilisateur]`
- `FTP_PASSWORD` : `[mot de passe]`
- `FTP_PORT` : `21`
- `FTP_LOCAL_DIR` : `./public/`
- `FTP_SERVER_DIR` : `/home/[nom du répertoire]/www/`

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
          hugo-version: '0.145.0'
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

      - name: SFTP Deploy
        uses: SamKirkland/FTP-Deploy-Action@v4.3.4
        with:
          server: ${{ secrets.FTP_HOSTNAME }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          port: ${{ secrets.FTP_PORT }}
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

### Méthode SSH

A partir de l'hébergement Pro, on peut utiliser le SSH qui est la méthode recommandée. Cependant, cela demande un pré-requis technique pour le mettre en place donc si vous ne savez pas utiliser un terminal de lignes de commande, restez sur la méthode FTP.

Tout d'abord, dans l'interface OVH, veillez à avoir un utilisateur avec le SSH activé.

Ensuite, il vous faudra générer une clé SSH (`ssh-keygen -t ed25519 -C "votre-adresse-email@exemple.fr"`)

Enfin, ajoutez votre clé SSH aux clés autorisées avec : `ssh-copy-id -i [NOM DE VOTRE CLE] [UTILISATEUR FTP]@[NOM HOTE FTP]`. Exemple : `ssh-copy-id -i ~/.ssh/id_ed25519 user@ssh.cluster042.hosting.ovh.net`

Une fois cela fait, vérifiez que la connexion via SSH fonctionne avec : `ssh -i [NOM DE VOTRE CLE] [UTILISATEUR FTP]@[NOM HOTE FTP]`. Exemple : `ssh -i ~/.ssh/id_ed25519 user@ssh.cluster042.hosting.ovh.net`.

Aller sur GitHub, dans "Settings", "Secrets and variables", "Actions", puis dans l'onglet "Secrets", définissez les *repository secrets* suivants.
- `SSH_HOST` : `ssh.[adresse du cluster].hosting.ovh.net`
- `SSH_PORT` : `22`
- `SSH_PRIVATE_KEY` : `[la clé privée SSH]`
- `SSH_USER` : `[nom de l'utilisateur]`
- `SSH_WORKDIR` : `/home/[nom du répertoire]/www`

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
          hugo-version: '0.145.0'
          extended: true

      - name: Setup Node
        uses: actions/setup-node@v3
        with:
          node-version: 'lts/*'
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
