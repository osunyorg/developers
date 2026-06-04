---
title: GitLab
weight: 50
---

Que ce soit pour publier en SSH ou en FTP, il faut utiliser le CI/CD de GitLab, qui permet d'exécuter une tâche à chaque modification du site.

## SSH

Assurez-vous d'avoir toutes l'informations nécessaires :
- `SSH_HOST` : `[serveur hôte ci-dessus]`
- `SSH_PORT` : `[port SSH (par défaut 22)]`
- `SSH_KNOWN_HOSTS` : le retour de la commande `ssh-keyscan [SSH_HOST]`
- `SSH_PRIVATE_KEY` : `[la clé privée SSH]`
- `SSH_USER` : `[nom de l'utilisateur ci-dessus]`
- `SSH_WORKDIR` : `[dossier vers le site (exemple : /var/www)]`

### Configuration du déploiement

Aller sur GitLab, sur la page de votre projet, dans "Paramètres", "CI/CD", "Variables", puis définissez les *variables* ci-dessus. A noter que `SSH_PRIVATE_KEY` et `SSH_KNOWN_HOSTS` doivent être de type "Fichier".

Créer l'action automatisée dans le fichier `.gitlab-ci.yml`

```yaml
image: hugomods/hugo:debian-node-git-0.159.2
variables:
  GIT_SUBMODULE_STRATEGY: recursive

publish:
  before_script:
  - apt-get update -y
  - apt-get -y install ssh rsync
  - eval $(ssh-agent -s)
  - chmod 400 "$SSH_PRIVATE_KEY"
  - ssh-add "$SSH_PRIVATE_KEY"
  - mkdir -p ~/.ssh
  - chmod 700 ~/.ssh
  - cp "$SSH_KNOWN_HOSTS" ~/.ssh/known_hosts
  - chmod 644 ~/.ssh/known_hosts
  script:
  - yarn install
  - yarn osuny build
  - rsync -avz --delete -e "ssh -p 22" ./public/ $SSH_USER@$SSH_HOST:$SSH_WORKDIR
  artifacts:
    paths:
    - public
  only:
  - main
```

## FTP

> [!WARNING]
> Work in progress

Assurez-vous d'avoir toutes l'informations nécessaires :
- `FTP_HOSTNAME` : `[serveur hôte ci-dessus]`
- `FTP_USERNAME` : `[nom de l'utilisateur ci-dessus]`
- `FTP_PASSWORD` : `[mot de passe ci-dessus]`
- `FTP_PORT` : `21`
- `FTP_LOCAL_DIR` : `./public/`
- `FTP_SERVER_DIR` : `[dossier vers le site]/`

*NOTE : Ne pas oublier le `/` à la fin du `FTP_SERVER_DIR`. Exemple : `/var/www/`*

## Configuration du déploiement

Aller sur GitLab, sur la page de votre projet, dans "Paramètres", "CI/CD", "Variables", puis définissez les *variables* ci-dessus.

Créer l'action automatisée dans le fichier `.gitlab-ci.yml`

```yaml
TODO
```
