---
title: "Contribuer au thème"
weight: 4
---

*Partager des améliorations du thème Osuny Hugo AAA avec toute la communauté*

![](/images/home/theme.jpg)

La méthode de travail pour intervenir sur le thème en dehors de l'équipe noesya n'est pas encore bien calée et documentée.
Contributions bienvenues !
## Forker

Dans Github, cliquez sur "Fork" pour créer votre propre version du thème.
TODO préparer les choses pour pouvoir travailler sur les données "example" directement dans le thème.
## Installer le thème

```bash
git clone git@github.com:osunyorg/theme.git --recurse-submodules
cd theme
yarn
hugo server
```

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


## Contrôle qualité et non-régression

Mise à part un contexte de hotfix très maîtrisé et sans possibilité d'effets de bord, il faut faire une PR pour modifier le thème. Avant de soumettre la PR en review, il faut s'assurer d'avoir bien testé son code sur le site d'exemple en local et en staging sur example.osuny.org.

Si une modification majeure est apportée (modification du DOM, de la structure, suppression d'une classe ou d'un partiel...) il faut bien vérifier dans tous les sites que l'élément supprimé ou modifié n'est pas overridé dans un autre site. Si c'est le cas, il faut bien s'assurer de gérer la modification des cas particuliers avant de faire une release et déployer le thème sur tous les sites.

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
Sites touchy à migrer
- https://osuny.org (et son thème)
- https://www.aliceetlescryptotrolls.org
- https://www.cmjnrvb.net
- https://works.noesya.coop/ (et son thème)
