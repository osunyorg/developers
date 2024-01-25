---
title: Deuxfleurs
---

❤️

Pour publier chez Deuxfleurs, il faut utiliser une action GitHub, c’est à dire une tâche qui va s’exécuter à chaque modification du site.

## Accès au Guichet

Tout d'abord, si la première fois que vous souhaitez héberger votre site avec Deuxfleurs, il vous faudra les contacter en envoyant un message à coucou[@]deuxfleurs.fr

Une fois cela fait, vous pourrez accéder à votre espace sur https://guichet.deuxfleurs.fr

## Récupération des identifiants S3

Pour déployer votre site, vous aurez besoin de vos identifiants S3 présents dans Guichet, dans la partie "Garage", "Vos identifiants", "S3". C'est à dire ici : https://guichet.deuxfleurs.fr/garage/key

Gardez sous la main votre **Identifiant de clé** et votre **Clé secrète**.

![image](https://github.com/noesya/osuny-developers/assets/7761386/a3e71c70-3ba5-46a4-b6b2-856efaf1169d)

## Ajout du site au Guichet

Dans Guichet, aller dans la partie "Garage", "Mes sites webs" puis ajouter votre site web avec le nom de domaine que vous avez acheté au préalable.

Notez l'**ID** qui vous est retourné, vous en aurez besoin.

![image](https://github.com/noesya/osuny-developers/assets/7761386/33abdf14-7a59-4fbd-8aac-64422d55ece5)

## Configuration DNS

Il vous faudra paramétrer la zone DNS de votre nom de domaine pour qu'il redirige vers Deuxfleurs.
- Si le domaine que vous avez choisi est un domaine racine (ex: monsite.fr), vous devrez créer un `ALIAS` vers `garage.deuxfleurs.fr`.
- Si le domaine que vous avez choisi est un sous-domaine (ex: www.monsite.fr), vous devrez créer un `CNAME` vers `garage.deuxfleurs.fr`.

## Configuration du déploiement

### URL cible

Aller dans votre repository GitHub contenant votre site web. Naviguez jusqu'au fichier `config/_default/config.yaml` puis modifiez-le pour rajouter la section dédiée au déploiement.

```yaml
deployment:
  targets:
    - name: "production"
      URL: "s3://<votre ID de site web>?endpoint=garage.deuxfleurs.fr&s3ForcePathStyle=true&region=garage"
```

*Pensez bien à modifier `<votre ID de site web>` par l'ID noté précédemment dans l'ajout du site au Guichet*

### Identifiants

Aller sur GitHub, dans "Settings", "Secrets and variables", "Actions", puis dans l'onglet "Secrets", définissez les *repository secrets* suivants.
- `AWS_ACCESS_KEY_ID` : `[votre identifiant de clé]`
- `AWS_SECRET_ACCESS_KEY` : `[votre clé secrète]`

### Action GitHub

Créer l'action automatisée dans le fichier `.github/workflows/deploy.yml`

```yaml
name: Déploiement Garage

on:
  push:
    branches: [ "main" ]

jobs:
  build_and_deploy:
    name: Compilation du site Hugo et déploiement Deux fleurs
    runs-on: ubuntu-latest
    steps:

    - name: Récupération des données
      uses: actions/checkout@v3
      with:
        submodules: true

    - name: Installation de Node
      uses: actions/setup-node@v3
      with:
        node-version: '16'
        cache: 'yarn'

    - name: Installation des dépendances JavaScript
      run: yarn install --frozen-lockfile

    - name: Installation de Hugo
      uses: peaceiris/actions-hugo@v2.6.0
      with:
        hugo-version: 'latest'
        extended: true

    - name: Compilation du site
      run: hugo -e production --minify

    - name: Déploiement Garage
      run: hugo deploy --force --maxDeletes -1
      env:
        AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
        AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}

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
