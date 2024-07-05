---
title: Deuxfleurs
---


[Deuxfleurs](https://www.deuxfleurs.fr) ❤️ est la solution d'hébergement intégrée par défaut dans Osuny.

## Configuration DNS

Il vous faudra paramétrer la zone DNS de votre nom de domaine pour qu'il redirige vers Deuxfleurs.

Le pointage se fait toujours vers `production.osuny.site.` (avec le point à la fin)

{{< callout type="info" >}}
  Le domaine de Deuxfleurs est `garage.deuxfleurs.fr`, mais nous ne l'utilisons pas pour des raisons de robustesse.
  Si une attaque en déni de service (DoS) bloquait tous les sites, nous pourrions déployer une solution de repli en changeant le pointage DNS, à la fois au niveau de Deuxfleurs et au niveau de noesya.
{{< /callout >}}

### Sans www 

Cela concerne le domaine nu, appelé aussi apex.

Si le domaine que vous avez choisi est un domaine racine (ex: monsite.fr), vous devrez créer un `ALIAS` vers `production.osuny.site.`. Si votre registrar (Gandi, OVH...) ne permet pas l'alias à l'APEX, il faudra faire une redirection vers www en http et https.

```DNS
www IN ALIAS production.osuny.site.
```

### Avec www
  
Cela concerne aussi tout autre sous-domaine que www.

Si le domaine que vous avez choisi est un sous-domaine (ex: www.monsite.fr), vous devrez créer un `CNAME` vers `production.osuny.site.`.

```DNS
www IN CNAME production.osuny.site.
```

## Mise en ligne manuelle (obsolète)

Cette partie de la documentation est obsolète parce que tout se fait automatiquement dans Osuny avec l'API Deuxfleurs.

### Gestion avec Guichet


#### Accès au Guichet

Tout d'abord, si la première fois que vous souhaitez héberger votre site avec Deuxfleurs, il vous faudra les contacter en envoyant un message à coucou[@]deuxfleurs.fr

Une fois cela fait, vous pourrez accéder à votre espace sur https://guichet.deuxfleurs.fr

#### Récupération des identifiants S3

Pour déployer votre site, vous aurez besoin de vos identifiants S3 présents dans Guichet, dans la partie "Garage", "Vos identifiants", "S3". C'est à dire ici : https://guichet.deuxfleurs.fr/garage/key

Gardez sous la main votre **Identifiant de clé** et votre **Clé secrète**.

![image](https://github.com/noesya/osuny-developers/assets/7761386/a3e71c70-3ba5-46a4-b6b2-856efaf1169d)

#### Ajout du site au Guichet

Dans Guichet, aller dans la partie "Garage", "Mes sites webs" puis ajouter votre site web avec le nom de domaine que vous avez acheté au préalable.

Notez l'**ID** qui vous est retourné, vous en aurez besoin.

![image](https://github.com/noesya/osuny-developers/assets/7761386/33abdf14-7a59-4fbd-8aac-64422d55ece5)


### Configuration du déploiement
 
Cette partie de la documentation est obsolète parce que tout est généré automatiquement par Osuny.

Pour publier chez Deuxfleurs, il faut utiliser une action GitHub, c’est à dire une tâche qui va s’exécuter à chaque modification du site.

#### URL cible

Aller dans votre repository GitHub contenant votre site web. Naviguez jusqu'au fichier `config/_default/config.yaml` puis modifiez-le pour rajouter la section dédiée au déploiement.

```yaml
deployment:
  targets:
    - name: "production"
      URL: "s3://<votre ID de site web>?endpoint=garage.deuxfleurs.fr&s3ForcePathStyle=true&region=garage"
```

*Pensez bien à modifier `<votre ID de site web>` par l'ID noté précédemment dans l'ajout du site au Guichet*

#### Identifiants

Aller sur GitHub, dans "Settings", "Secrets and variables", "Actions", puis dans l'onglet "Secrets", définissez les *repository secrets* suivants.
- `AWS_ACCESS_KEY_ID` : `[votre identifiant de clé]`
- `AWS_SECRET_ACCESS_KEY` : `[votre clé secrète]`

#### Action GitHub

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
