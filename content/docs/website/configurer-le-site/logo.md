---
title: Logo et favicon
weight: 1
---

## Logo

Remplacer le fichier `static/assets/images/logo.svg`  
Dans le cas où on n'aurait pas de logo svg il est possible de mettre tout autre format et il faut alors modifier le fichier de config dans `config/_defaults/config.yaml` pour ajouter les lignes suivantes :

```yml
params:
  logo:
    header: "/assets/images/logo.png"
    footer: "/assets/images/logo.png"
```

## Favicon

Il existe plusieurs manière d'ajouter un favicon sur un site Osuny. La plus simple est d'ajouter un fichier `favicon.png` ou `favicon.ico` dans le dossier `static/assets/images/favicons/`.
Si vous souhaitez ajouter plus de formats de favicon, vous pouvez ajouter les fichiers si dessous au dossier `static/assets/images/favicons/` :

- `apple-touch-icon.png`
- `favicon-16x16.png`
- `favicon-32x32.png`
- `favicon.ico`
- `favicon.png`
- `safari-pinned-tab.svg`