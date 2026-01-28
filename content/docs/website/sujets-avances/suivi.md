---
title: Intégrer les outils de suivi
description: >-
  Ajouter un outil d'analyse et suivre les performances d'un site Osuny.
weight: 1
---

## Plausible

Plausible est un outil d'analyse intégré dans Osuny, il peut être activé à la demande.

Pour en savoir plus : [Osuny ❤️ Plausible](https://www.osuny.org/actualites/2022-09-27-osuny-plausible/)

## Matomo

Une fois le script Matomo obtenu, l'intégration dans le site Osuny se fait à deux endroits.

### 1. Ajout du script

Le script fourni par Matomo doit être intégré dans un overide du fichier `script.html` du footer : `layouts/partials/footer/script.html`.
En l'état, ça ne fonctionne pas.

### 2. Ajout de la configuration

Dans le fichier `config/_default/config.yaml`, il faut renseigner un nouveau paramètre :

```yaml
params:
  additional_csp: cdn.matomo.cloud url-du-site.matomo.cloud 'unsafe-inline'

```

ou bien :

```yaml
params:
  additional_csp: matomo.sig.url-du-site.fr 'unsafe-inline'

```


NB : le script ne doit pas contenir de cookies, Osuny n'en utilise pas.
