---
title: Mettre à jour le thème
weight: 5
description: >
  Le thème évolue fréquemment, et il est nécessaire de le maintenir à jour
---

## Pourquoi ?

Le thème prend en charge les données générées par le back-office Osuny.
Quand ces données évoluent, il faut que le thème suive.
Par exemple, nous venons d'ajouter un bloc "Son" permettant d'intégrer des enregistrements audio dans les sites.
Si quelqu'un utilise ce bloc et que vous n'utilisez pas la version à jour du thème, cela risque de bloquer la compilation.
Votre site en production fonctionnera toujours, mais les mises à jour de contenu ne se publieront plus tant que le thème ne sera pas à jour.

## Comment mettre à jour le thème ?
Pour récupérer la dernière version du thème :
```bash
yarn osuny update-theme
```

Lancer ensuite le site en local, pour vérifier que tout se passe bien.
Si vous avez des overrides HTML dans `/layouts`, vérifiez qu'ils sont toujours fonctionnels.

## Branches, pre-release et release

Avant une release du thème, une branche est déployée. Les différentes PR d'évolutions et de maintenance sont merge à l'intérieur de cette branche. Ensuite une pre-release est déployée, un temps est alloué aux développeuses et développeurs qui maintiennent les sites pour s'assurer des impacts de ces évolutions sur leurs sites. 


## Et le template ?

La mise à jour du template n'est en général pas nécessaire.
Si vraiment vous voulez le faire, voilà la méthode.
Malheureusement, cette méthode génère des conflits, qu'il faut traiter à la main.

Pour faire la mise à jour du template :
```bash
git remote add template git@github.com:noesya/osuny-hugo-template-aaa.git
git fetch --all
git merge template/main --allow-unrelated-histories
```
