---
title: Recherche
weight: 1
---

## Activer pagefind

La recherche sur Osuny s'appuie sur la librairie [pagefind](https://pagefind.app/).

Pour activer la recherche :

```YAML(config/_default/config.yaml)
params:
  search:
    active: true
```

## Recherche multisites

Pour activer la recherche multisites :

```JS(assets/js/main.js)
import './theme/';

window.osuny.pagefindOptions = window.osuny.pagefindOptions || {};

window.osuny.pagefindOptions.mergeIndex = [
    {
        bundlePath: "https://example.osuny.org/pagefind",
        showSubResults: true
    },
    {
        bundlePath: "https://conservatoire-pierre-barbizet.inseamm.osuny.site/pagefind",
        showSubResults: true
    }
];
```

> ⚠️ Si les sites ont des domaines différents, il faut configurer les CORS pour que les sites autorisent la recherche depuis un autre site : [voir la documentation de pagefind](https://pagefind.app/docs/multisite/#cross-origin-indexes).