---
title: Infomaniak
weight: 2
---

Pour publier chez Infomaniak, il faut utiliser une action GitHub, c'est à dire une tâche qui va s'exécuter à chaque modification du site.

Tous les hébergements d'Infomaniak proposent du SSH, sauf l'hébergement 10 Mo offert avec le nom de domaine. Pour celui, la méthode en FTP est obligatoire.

## Configuration du déploiement

### FTP (hébergement gratuit)

Si ce n'est pas déjà fait, créez un compte FTP sur votre hébergement.

![Ajouter un compte FTP sur Infomaniak](/images/infomaniak/ftp1.png)

Les identifiants seront utilisés pour la tâche GitHub (`FTP_USERNAME` et `FTP_PASSWORD`)

![Ajouter un compte FTP sur Infomaniak](/images/infomaniak/ftp2.png)

Une fois validé, vous verrez le serveur hôte, il s'agira de la valeur pour la variable `FTP_HOSTNAME`

![Serveur FTP hôte sur Infomaniak](/images/infomaniak/ftp3.png)

Aller sur GitHub, dans "Settings", "Secrets and variables", "Actions", puis dans l'onglet "Secrets", définissez les *repository secrets* suivants.
- `FTP_HOSTNAME` : `[serveur hôte ci-dessus]`
- `FTP_USERNAME` : `[nom de l'utilisateur ci-dessus]`
- `FTP_PASSWORD` : `[mot de passe ci-dessus]`
- `FTP_PORT` : `21`
- `FTP_LOCAL_DIR` : `./public/`
- `FTP_SERVER_DIR` : `./`

Créer l'action automatisée dans le fichier `.github/workflows/deploy.yml`

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

      - name: SFTP Deploy
        uses: SamKirkland/FTP-Deploy-Action@v4.3.4
        with:
          server: ${{ secrets.FTP_HOSTNAME }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          local-dir: ${{ secrets.FTP_LOCAL_DIR }}
          server-dir: ${{ secrets.FTP_SERVER_DIR }}
```

### SSH (hébergement payant)

Dans le cas de l'hébergement payant, on peut utiliser le SSH qui est la méthode recommandée. Cependant, cela demande un pré-requis technique pour le mettre en place donc si vous ne savez pas utiliser un terminal de lignes de commande, restez sur la méthode FTP.

Tout d'abord, créer un utilisateur FTP+SSH (voir la création de l'utilisateur FTP au-dessus).

La variable `FTP_HOSTNAME` correspondra à `SSH_HOST` et `FTP_USERNAME` à `SSH_USER`.

Ensuite, il vous faudra générer une clé SSH (`ssh-keygen -t ed25519 -C "votre-adresse-email@exemple.fr"`)

Enfin, ajoutez votre clé SSH aux clés autorisées avec : `ssh-copy-id -i [NOM DE VOTRE CLE] [UTILISATEUR FTP]@[NOM HOTE FTP]`. Exemple : `ssh-copy-id -i ~/.ssh/id_ed25519 user@mon-domaine.fr`

Une fois cela fait, vérifiez que la connexion via SSH fonctionne avec : `ssh -i [NOM DE VOTRE CLE] [UTILISATEUR FTP]@[NOM HOTE FTP]`. Exemple : `ssh -i ~/.ssh/id_ed25519 user@mon-domaine.fr`.

Aller sur GitHub, dans "Settings", "Secrets and variables", "Actions", puis dans l'onglet "Secrets", définissez les *repository secrets* suivants.
- `SSH_HOST` : `[serveur hôte ci-dessus]`
- `SSH_PORT` : `22`
- `SSH_PRIVATE_KEY` : `[la clé privée SSH]`
- `SSH_USER` : `[nom de l'utilisateur ci-dessus]`
- `SSH_WORKDIR` : `/web`

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
```

## Certificat SSL

Pour avoir de l'HTTPS, il vous faudra commander un certificat SSL. Aller dans la partie Domaines et sélectionner votre nom de domaine.

Aller ensuite dans "Commander un certificat SSL"

![Commander un certificat SSL sur Infomaniak](/images/infomaniak/ssl1.png)

Sélectionner le certificat gratuit Let's Encrypt

![Certificat Let's Encrypt sur Infomaniak](/images/infomaniak/ssl2.png)

Assigner également le certificat au www

![SSL sur les alias sur Infomaniak](/images/infomaniak/ssl3.png)

Cela peut prendre un certain moment avant d'être changé.

Ensuite, configurer le serveur web avec la documentation suivante : [Apache](/docs/website/mettre-en-ligne/serveurs-web/apache/).