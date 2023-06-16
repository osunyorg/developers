---
title: Netlify
---

En connectant Netlify au référentiel GitHub, tout est pris en charge automatiquement.

[Suivre la documentation officielle](https://docs.netlify.com/welcome/add-new-site/#import-from-an-existing-repository)

## Gérer la récupération du submodule

Si vous avez créé votre site à partir du template, le submodule du thème est récupéré en HTTPS sans problème. Or, il est possible que vous ayez ce lien en SSH (travail interne, ou fork personnel du thème).

Dans ce cas-là, vous avez 2 solutions :
- Changer l'URL du submodule en HTTPS
- Ajouter la deploy key de Netlify pour récupérer le submodule sans problème.

### Changer l'URL du submodule en HTTPS

Pour cela, modifiez le fichier `.gitmodules` et modifier l'URL du submodule en question. Par exemple :
- avant : `git@github.com:noesya/osuny-hugo-theme-aaa.git`
- après : `https://github.com/noesya/osuny-hugo-theme-aaa.git`

Puis lancer la commande `git submodule sync --recursive` et commit les changements.

### Ajouter la deploy key

Pour récupérer la deploy key, aller sur Netlify, puis aller dans les "Site settings" de votre site.

- Aller dans la partie "Build & deploy", scrollez jusqu'au bloc "Deploy key", puis cliquer sur "Generate public deploy key".
- Copier la clé dans votre presse-papiers.

Aller maintenant sur le repository de votre submodule sur GitHub.

- Aller dans "Settings", puis dans la section "Security", aller dans "Deploy keys".
- Cliquer sur "Add deploy key", mettez en titre le nom de votre site, et coller la deploy key.
- Laisser la case "Allow write access" vide et sauvegarder.

Vous pouvez déployer votre site !

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
