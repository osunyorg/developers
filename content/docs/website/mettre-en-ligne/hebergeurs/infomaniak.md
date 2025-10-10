---
title: Infomaniak
---

Pour publier chez Infomaniak, il faut utiliser une action GitHub, c'est à dire une tâche qui va s'exécuter à chaque modification du site. Tous les hébergements d'Infomaniak proposent du SSH, sauf l'hébergement 10 Mo offert avec le nom de domaine. Pour celui-ci, la méthode FTP est obligatoire.

## Création du compte

Si ce n'est pas déjà fait, créez un compte FTP sur votre hébergement.

![Ajouter un compte FTP sur Infomaniak](ftp1.png)

Les identifiants seront utilisés pour la tâche GitHub (`FTP_USERNAME` et `FTP_PASSWORD`)

![Ajouter un compte FTP sur Infomaniak](ftp2.png)

Une fois validé, vous verrez le serveur hôte, il s'agira de la valeur pour la variable `FTP_HOSTNAME`

![Serveur FTP hôte sur Infomaniak](ftp3.png)

## Déploiement en SSH (recommandé)

Dans le cas de l'hébergement payant, on peut utiliser le SSH qui est la méthode recommandée. Cependant, cela demande un pré-requis technique pour le mettre en place donc si vous ne savez pas utiliser un terminal de lignes de commande, utilisez la méthode FTP ci-après.

### Créer la clé SSH

Il vous faudra générer une clé SSH en local, sur votre ordinateur :
```bash
ssh-keygen -t ed25519
```

> [!WARNING]
> Il ne faut pas générer la clé en RSA, car Infomaniak ne les autorise pas. Source : https://www.infomaniak.com/fr/support/faq/2054/se-connecter-avec-cle-ssh

Le script va demander un nom de clé, que l'on désignera ici par `KEY_NAME`.
Une bonne pratique peut être de mettre le nom du site référentiel, avec des underscores (_) et pas des tirets, suivi de `_osuny`.

```bash { filename="Exemple de KEY_NAME" }
reakt_site_osuny
```

Ensuite, le script demande un mot de passe.
On peut ne pas en mettre, puis confirmer l'absence.


Cela génère une clé à l'endroit où l'on exécute le script, sous la forme de 2 fichiers (clé publique + clé privée). 
Peu importe ce qu'on en fait, du moment que les fichiers restent confidentiels.


Puis il faut l'envoyer sur le serveur afin de l'ajouter aux clés autorisées avec : 
```bash {filename="Description de la commande"}
ssh-copy-id -i [KEY_NAME] [SSH_USER]@[SSH_HOST]
```
Le nom de la clé est le nom du fichier que l'on vient de rentrer, dans le même répertoire

```bash {filename="Commande avec des données d'exemple"}
ssh-copy-id -i reakt_site_osuny username@server.com
```

Une fois cela fait, vérifiez que la connexion via SSH fonctionne avec :
```bash {filename="Description de la commande"}
ssh -i [KEY_NAME] [SSH_USER]@[SSH_HOST]
````

```bash {filename="Commande avec des données d'exemple"}
ssh -i reakt_site_osuny username@server.com
```

Si tout va bien, la commande ne demande pas de mot de passe et nous connecte au serveur.


[En savoir plus sur ssh](https://www.ssh.com/academy/ssh/keygen)

### Paramétrer Github

Aller sur GitHub, dans "Settings", "Secrets and variables", "Actions", puis dans l'onglet "Secrets", définissez les *repository secrets* suivants.

| Nom | Description | Exemple |
|---|---|---|
| SSH_HOST | serveur hôte ci-dessus | server.com |
| SSH_PORT | Numéro de port | 22 |
| SSH_PRIVATE_KEY | la clé privée SSH | exemple ci-dessous |
| SSH_USER | nom de l'utilisateur ci-dessus | username | 
| SSH_WORKDIR | Chemin sur le serveur | /web ou /sites/nomdusite |

Le SSH_PRIVATE_KEY est le contenu de la clé privée (le fichier sans l'extension `.pub`).

``` {filename="Exemple de SSH_PRIVATE_KEY"}
-----BEGIN OPENSSH PRIVATE KEY-----
b3BlbnNzaC1rZXktdjEAAAAABG5vbmUAAAAEbm9uZQAAAAAAAAABAAAAaAAAABNlY2RzYS
1zaGEyLW5pc3RwMjU2AAAACG5pc3RwMjU2AAAAQQR9WZPeBSvixkhjQOh9yCXXlEx5CN9M
yh94CJJ1rigf8693gc90HmahIR5oMGHwlqMoS7kKrRw+4KpxqsF7LGvxAAAAqJZtgRuWbY
EbAAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBH1Zk94FK+LGSGNA
6H3IJdeUTHkI30zKH3gIknWuKB/zr3eBz3QeZqEhHmgwYfCWoyhLuQqtHD7gqnGqwXssa/
EAAAAgBzKpRmMyXZ4jnSt3ARz0ul6R79AXAr5gQqDAmoFeEKwAAAAOYWpAYm93aWUubG9j
YWwBAg==
-----END OPENSSH PRIVATE KEY-----
```


### Créer l'action

Créer l'action automatisée dans le référentiel Git du site.

```yaml {filename=".github/workflows/infomaniak.yml"}
name: Infomaniak

on:
  push:
    branches:
      - main  # Set a branch to deploy
jobs:
  deploy:
    runs-on: ubuntu-latest
    concurrency:
      group: ${{ github.workflow }}
      cancel-in-progress: false # Can cause difficulties to publish when lots of changes are done
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

## Déploiement en FTP

Le FTP est disponible dans tous les hébergements, même gratuits.
Cette méthode est beaucoup plus lente que le SSH.

### Paramétrer Github

Aller sur GitHub, dans "Settings", "Secrets and variables", "Actions", puis dans l'onglet "Secrets", définissez les *repository secrets* suivants.

| Nom | Description | Exemple |
|---|---|---|
| FTP_HOSTNAME | serveur hôte ci-dessus | TODO |
| FTP_USERNAME | nom de l'utilisateur ci-dessus | TODO |
| FTP_PASSWORD | mot de passe ci-dessus | TODO |
| FTP_PORT| Port | 21 |
| FTP_LOCAL_DIR | Chemin des fichiers compilés | ./public/ |
| FTP_SERVER_DIR | Chemin sur le serveur | ./ |

Le chemin des fichiers compilés se situe sur l'instance Ubuntu déployée par Github pour exécuter l'action automatisée.

### Créer l'action

Créer l'action automatisée dans le référentiel Git du site.

```yaml {filename=".github/workflows/infomaniak.yml"}
name: Infomaniak

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
          node-version: 'lts/*'
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

## Certificat SSL

Pour avoir une connexion sécurisée en HTTPS, il vous faudra commander un certificat SSL. 
Aller dans la partie Domaines et sélectionner votre nom de domaine.

Aller ensuite dans "Commander un certificat SSL"

![Commander un certificat SSL sur Infomaniak](ssl1.png)

Sélectionner le certificat gratuit Let's Encrypt

![Certificat Let's Encrypt sur Infomaniak](ssl2.png)

Assigner également le certificat au www

![SSL sur les alias sur Infomaniak](ssl3.png)

Cela peut prendre un certain moment avant d'être changé.

Ensuite, configurer le serveur web avec la documentation suivante : [Apache](/docs/website/mettre-en-ligne/serveurs-web/apache/).
