---
title: Tailles d'images
weight: 3
---

## Adaptation des images

osuny fournit des images en fonction des contextes d'usages : 
- dimension de l'écran
- ratio de pixels
- support des formats du navigateur (`webp` ou `jpg/png` ?)

Les images sont fournies par le serveur. Ça veut dire qu'elles ne sont pas redimensionnées côté client : on ne charge que ce dont on a besoin.

### Formats

Les images bitmap sont servies en plusieurs formats :
- `jpg` ou `png`
- `webp`

Le thème intègre une gestion des images adaptatives sur trois breakpoints spécifiques : 

| nom | largeur minimum |
|-----------|------|
| `mobile`  | 0    |
| `tablet`  | 768  |
| `desktop` | 1280 |

Vous pouvez modifier ces tailles par défault : 

```
params:
  image_sizes:
    _default:
      hero:
        mobile:   400
        tablet:   800
        desktop:  750
      layouts:
        alternate:
          mobile:   360
          tablet:   555
          desktop:  520
```

Chaque nature éditoriale peut définir la taille de ses images en fonction de ses contextes : 

```
params:
  image_sizes:
    sections:
      events:
        layouts:
          grid:
            mobile: 302
            tablet:  400
            desktop: 575
```

## Tailles additionnelles

Dans certains cas, les trois largeurs par défaut sont insuffisantes, notamment dans le cas de très grande image (pleine largeur). Si on veut gérer une qualité suffisante sur un écran de 2048px de large, on doit pouvoir définir un nouveau `breakpoint`.

On peut définir un nouveau breakpoint par sa règle et la dimension de l'image attendue : 

```
rule: "min-width: 1560px"
size: 1792
```

Par exemple : 

```
image_sizes:
  sections:
      home:
        hero:
          mobile: 480
          tablet: 1200
          desktop: 1440
          additionnal:
            - rule: "min-width: 1560px"
              size: 1792
```

Il n'y a pas de limite de nombre de breakpoints, mais attention, à chaque breakpoint, de nouvelles balises sont ajoutées.

La configuration d'exemple ci-dessus, génère deux balises html supplémentaires.

```
  <source media="(min-width: 1560px)" type="image/webp" srcset="https://osuny-1b4da.kxcdn.com/s2bfeqenr3nmqqzqa8qkx8fu0y5x?format=webp&amp;width=1792&amp;height=0&amp;&amp;fit=inside&amp;quality=80,
            https://osuny-1b4da.kxcdn.com/s2bfeqenr3nmqqzqa8qkx8fu0y5x?format=webp&amp;width=3584&amp;height=0&amp;&amp;fit=inside&amp;quality=50 2x">
  <source media="(min-width: 1560px)" srcset="https://osuny-1b4da.kxcdn.com/s2bfeqenr3nmqqzqa8qkx8fu0y5x?width=1792&amp;height=0&amp;&amp;fit=inside&amp;quality=80,
            https://osuny-1b4da.kxcdn.com/s2bfeqenr3nmqqzqa8qkx8fu0y5x?width=3584&amp;height=0&amp;&amp;fit=inside&amp;quality=50 2x">

```

Cela peut paraître peu, mais si vous ajouter des `breakpoints` sur la grille des actualités, une page présentant 36 actualités aura donc 72 balises supplémentaires pour chaque `breakpointà. 