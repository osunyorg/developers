---
title: HTML
weight: 2
---

Vous pouvez remplacer n'importe quel fichiers du thème grace à Hugo.
Quand le site compile, Hugo va cherche le fichier à la racine avant celui du thème.
Par exemple si vous souhaitez modifier l'en-tête (`hero`) des actualités, vous pouvez créer le fichier

```
/layouts/partials/posts/hero-single.html
```

...qui viendra remplacer/overrider le fichier du thème.

```
themes/osuny/layouts/partials/posts/hero-single.html
```

{{< callout type="warning" >}}
  Attention, en décidant d'écraser un fichier HTML, vous ne bénéficierez plus des mises à jour de ce fichier. Il faudra alors mettre à jour manuellement le fichier si le fichier original du thème subit des modifications.
{{< /callout >}}
