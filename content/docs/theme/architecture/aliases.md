---
title: "Alias"
---

## Problématique

### Avant hugo 0.155.0

On gérait les alias avec des paths absolues. Par exemple : 

```
url: "/fr/chemin/actuel/de/la/page/"
aliases:
  - "/ancien/chemin/de/la/page/"
```

### Depuis hugo 0.155.0

À partir de 0.155, Hugo génére les alias absolus non pas à partir de la racine du site, mais à partir du site dans un contexte de langue.

Exemple : pour la page située dans `content/fr/pages/a-propos/index.html`, imaginons un alias `/nous/`

A côté de ça, il peut gérer les urls relatifs à l'emplacement du fichier

| Path type | File path | Alias | Server-relative path |
| :--- | :--- | :--- | :--- |
| page-relative | `content/examples/a.en.md` | `a-old` | `/en/examples/a-old/` |
| page-relative | `content/examples/a.en.md` | `../a-old` | `/en/a-old/` |
| site-relative | `content/examples/a.en.md` | `/a-old` | `/en/a-old/` |

Pour un site est monolingue il n'y a pas de problématique.

## Cas concret

Avec un alias comme : 

```
url: /fr/chemin/actuel/
aliases:
  - /ancien/chemin/
```

Hugo 0.155 va créer une redirection de `/fr/ancien/chemin/` et ne gère pas `/ancien/chemin`.

## Situation idéale

Générer les alias corrects. Demander à @bep.

## Solution proposée

Pour un site est multilingue : 

Comme hugo considère la racine "/" de l'alias comme le contexte de langue, il faut remonter l'alias d'un niveau pour être à la racine du site : 

`/ancien/chemin/` deviens alors `/../ancien/chemin/`


## Cas à tester

### Site monolingue

Tester les alias en racine.

### Site multilingue

#### 1. Alias racine

On veut rediriger : 

`/nous/` vers `/fr/a-propos/`

Piste : 

`
url: "/fr/a-propos/"
aliases:
  - /../nous/
`

#### 2. Alias dans la même langue

On veut rediriger : 

`/fr/nous/` vers `/fr/a-propos/`

Piste : 

`
url: "/fr/a-propos/"
aliases:
  - /../fr/nous/
`


#### 3. Alias d'une langue à une autre langue

Cas qui ne devrait normalement pas se produire sauf dans le cas d'une migration d'un site très mal rangé.

On veut rediriger : 

`/en/us/` vers `/fr/a-propos/`

Piste : 

`
url: "/fr/a-propos/"
aliases:
  - /../en/us/
`

