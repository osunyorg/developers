---
title: Mise à jour et versionning
aliases: 
  - /docs/theme/enjeux/maintenable
weight: 1
---

## Processus de mise à jour

### Définir l’évolution

C’est-à-dire bien remplir le modèle de Pull Request pour documenter l’évolution.

Définir le type de l’évolution :

- [ ] Nouvelle fonctionnalité
- [ ] Bug
- [ ] Ajustement
- [ ] Rangement

Définir le niveau d’impact de l’évolution : 
- [ ] Incidence faible 
- [ ] Incidence moyenne
- [ ] Incidence forte

> S’il y a des breakings changes, avertir les développeuses et développeurs d’un travail en cours sur cette PR pour leur permettre de mieux anticiper.


### Créer une pre-release

Définir le numéro de version [SEMVER](https://semver.org/lang/fr/).

Le nature et le niveau d’incidence de l’évolution vont déterminer le temps disponible avant la release de l’évolution. Par exemple, on peut compter une semaine de délai pour une modification majeure dans l’architecture des fichiers.

Certaines évolutions ne nécessitent pas de délai entre la pre-release et la release :
- Ajout de nouvelles fonctionnalités isolées
- Correctif mineur
- Réparation urgente

> Si d'autre modifications (comme des corrections de la pre-release) sont réalisées avant la realise, on nomme la nouvelle pre-release `x.x.x-beta.1` puis `x.x.x-beta.2`...

### Vérifications de non regression

Des outils sont en place pour s'assurer des non regressions :

1. Visuelle : utiliser `backstop-js` via [`osuny-cli`](https://github.com/osunyorg/cli)

2. Qualité : [lighthouse](https://lighthouse.noesya.coop/app/projects)

3. HTML : [w3c](https://w3c.noesya.coop/)


### Releases


#### Vérification manuelle

Si nécessaire en cas de modification majeure : 

1. Bien tester la/les feature(s) en local, en staging
2. Tester les features sur plusieurs environnements staging (à minima Deuxfleurs et Netlify)
  2.1 Tester desktop / mobile et navigateurs communs (FF, Edge, Safari, Chrome)
  2.2 Attention aux sites avec un darkmode
3. Demander (et attendre) une review sur la PR avant merge
4. Rechercher des effets de bord sur les sites avec override :
  4.1 Rechercher des références aux modifications sur l'organisation github (par exemple, changement d'un nom de classe dans le thème, ou d'un nom de partial)
  4.2 Noter les sites concernés pour les mettre à jour manuellement après le merge et avant la release
5. Une fois merge, déployer sur example.osuny.org
6. Re-tester
7. Si tout est ok et que les sites en étape 4 sont OK et en ligne, faire une release

#### Soft release

Si les évolutions sont majeures ou contiennent des breakings, on migre manuellement les sites avec d’importantes surcouches : https://github.com/topics/osuny-soft-release

Une fois les contrôles manuels effectués, on déploie de façon standard.

##### Liste des sites à vérifier manuellement lors de grosses releases

Si un site comporte de nombreux overrides, il faut le taggé dans la [liste des sites sensibles](https://github.com/topics/osuny-soft-release)

Les sous-thèmes :
- https://github.com/osunyorg/epv-theme
- https://github.com/osunyorg/cids-theme
- https://github.com/osunyorg/encommuns-theme
- https://github.com/osunyorg/sufficiency-theme
- https://github.com/osunyorg/bordeaux-theme
- https://github.com/osunyorg/rennes-theme

#### Déploiement

La pre-release est release, tous les sites Osuny sont mis à jour immédiatement.
