---
title: "Nouvelle version d'hugo.io"
weight: 10
---

## Étapes

### Utiliser la staging

Utiliser la version staging d'Osuny s'il y a des modifications des statics (`content` et `data`) et/ou de la structure des fichiers.

Lorsque les statics sont modifiés, il faut tester : 
- Les cas exhaustifs des blocs, incluant un test de chaque bloc vide
- La présence ou absence de clé
- Vérifier les fichiers statics orphelins. Faire attention à ne pas supprimer des orphelins nécessaires : une page spécifique non connectée à Osuny, comme dans [le jeu Alice par exemple](https://github.com/osunyorg/alice/blob/main/content/fr/jouer.html)

### Tester en local

- Utiliser le [CLI](https://github.com/osunyorg/cli) pour tester la compilation avec les commandes de `osuny test` et `osuny test-factory`
- Utiliser le [CLI](https://github.com/osunyorg/cli) pour tester les regressions visuelles avec `osuny backstop` et `osuny backstop-factory`
- Vérifier les overrides : Rechercher que les sites qui ont des overrides soient toujours compatibles. Si besoin il faut prévoir une soft-release de ces sites.

### Tester l'hébergement

#### Deuxfleurs

Modifier la github action pour changer la version d'hugo et tester sur un site hébergé chez deuxfleurs. D'abord en staging puis en production sur le [site d'example](https://github.com/osunyorg/example).

#### Netlify

Tester le [site d'example](https://github.com/osunyorg/example) sur netlify avec le bon numéro de version.

#### Autres hébergeurs

Réaliser le test de monter en version d'hugo sur des sites hébergés sur : 

- OVH
- Hébergement de Rennes
- Infomaniak

### Planifier le déploiement

Il faut prévoir un créneau où l'ensemble de l'équipe est disponible pour garantir le bon déroulement de la release. 

{{< callout type="warning" >}}
Le déploiement d'une version majeure n'est pas autorisée le vendredi : les sites sont recompilés la nuit, il faut être disponible le lendemain matin pour s'assurer que tous se portent bien.
{{</ callout >}}


#### Prévenir les développeuses et développeurs

- Alerter sur le déploiement de la nouvelle version
- Expliquer la monter en version et ses impacts
- Rappeler qu'il faut changer la version d'hugo locale sur leur machine

## Après la release

### Vérifier les versions des sites

Via l'interface en admin, s'assurer des bonnes compilations et versions des sites.

Les sites qui bénéficient d'une maintenance facturée doivent être vérifiés en premier.

### Informer

Informer les usagers que le déploiement est terminé.