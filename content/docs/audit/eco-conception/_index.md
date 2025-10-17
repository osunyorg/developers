---
title: Éco-conception
---

https://comnum.rennes.fr/declaration-decoconception/

## Critères

### 8.10 – Le service numérique tient-il compte des contraintes externes pour minimiser l’impact environnemental des calculs et transferts de données asynchrones ?

#### Caches

La durée du cache de KeyCDN (images et média) est d'un an.

```
cache-control: max-age=31556940
```

La durée des caches pour les `fonts`, `css` et `js` est d'un an.

```
## CACHE
deployment:
  matchers:
    - pattern: "^.+\\.(woff2|woff|svg|ttf|otf|eot|js|css)$"
      cacheControl: "max-age=31536000, no-transform, public"
      gzip: true
    - pattern: "^.+\\.(png|jpg|jpeg|gif|webp)$"
      cacheControl: "max-age=31536000, no-transform, public"
      gzip: false
```

#### Tâches automatisées (CRON)

Osuny groupe les traitements par lot de 10 minutes avant de générer les fichiers statics. Cela évite de générer les fichiers lors de multiples modifications d'une page.

Le nettoyage des données a lieu à 23h UTC+2

La mise à jour des publications scientifiques HAL a lieu à 1h UTC+2.

Le nettoyage des tous les sites et leur reconstruction si nécessaire ont lieu à 2h UTC+2.

La synchronisation automatique des sites ont lieu toutes les 10 minutes.


```JSON {filename="cron.json"}
"jobs": [
  {
    "command": "0 21 * * * rails auto:daily_care",
    "size": "S"
  },
  {
    "command": "0 23 * * * rails auto:update_hal",
    "size": "L"
  },
  {
    "command": "0 0 * * * rails auto:clean_and_rebuild_websites",
    "size": "L"
  },
  {
    "command": "*/10 * * * * rails auto:synchronize_websites",
    "size": "S"
  }
]
```
