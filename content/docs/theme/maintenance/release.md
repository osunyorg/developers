---
title: Mise à jour et déploiement
weight: 1
---

## Nommages et conventions Git

### Pull Request
- En français
- Un nom court mais clair
- Les commits sont squash dans les PR

### Branche
- En anglais
- En kebab-case: block-truc-fix-bug

### Commit
- En anglais
- À minima deux trois mots décrivant la modification 


## Review de code (PR)

Comment relire une grosse PR ?

- Tester en local sur example
- Vérifier les breakpoints
- Vérifier les états (full-width / ou pas full-width)


## Forker

Dans Github, cliquez sur "Fork" pour créer votre propre version du thème.
TODO préparer les choses pour pouvoir travailler sur les données "example" directement dans le thème.


## Déploiement d'une modification majeure

1. Bien tester la/les feature(s) en local, en staging
2. Tester les features sur plusieurs environnements staging (à minima Deuxfleurs et Netlify)
  2.1 Tester desktop / mobile et navigateurs communs (FF, Edge, Safari, Chrome)
  2.2 Attention aux sites avec un darkmode
3. Demander (et attendre) une review sur la PR avant merge
4. Rechercher des effets de bord sur les sites avec override :
  4.1 Récupérer tous les sites en local (commande magique git clone tous les sites osuny)
  4.2 Ouvrir le dossier avec tous les sites dans VS Code
  4.3 Rechercher des références aux modifications (par exemple, changement d'un nom de classe dans le thème, ou d'un nom de partial)
  4.4 Noter les sites concernés pour les mettre à jour manuellement après le merge et avant la release
5. Une fois merge, déployer sur example.osuny.org
6. Re-tester
7. Si tout est ok et que les sites en étape 4 sont OK et en ligne, faire une release



## Liste des sites à vérifier manuellement lors de grosses releases


Sites importants
- https://www.bonnesnotes.org
- https://www.cepir.info
- https://comnum.rennes.fr
- https://www.la-criee.org/fr/
- https://www.degrowthjournal.org
- https://www.fondation-jacques-rougerie.com/fr/
- https://ran-coper.fr
- https://www.re-akt.ch
- https://www.sinonvirgule.fr
- https://www.vincentgambardella.com
- https://africanfutures.mit.edu
- https://www.communication-democratie.org/fr/
- https://www.iut.u-bordeaux-montaigne.fr/
- https://dico.unric.org/fr/
- https://association.encommuns.net/
- https://www.encommuns.net/
- https://www.reseauexcellence.fr/fr/
  
Sites touchy à migrer
- https://osuny.org (et son thème)
- https://www.aliceetlescryptotrolls.org
- https://www.cmjnrvb.net
- https://works.noesya.coop/ (et son thème)

Sous-thèmes :
- https://github.com/osunyorg/epv-theme
- https://github.com/osunyorg/cids-theme
- https://github.com/osunyorg/encommuns-theme
- https://github.com/osunyorg/sufficiency-theme
- https://github.com/osunyorg/bordeaux-theme
- https://github.com/osunyorg/rennes-theme
