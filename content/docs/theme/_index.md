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
cd osuny-hugo-theme-aaa
yarn
hugo server
```

## Contrôle qualité et non-régression

Mise à part un contexte de hotfix très maîtrisé et sans possibilité d'effets de bord, il faut faire une PR pour modifier le thème. Avant de soumettre la PR en review, il faut s'assurer d'avoir bien testé son code sur le site d'exemple en local et en staging sur example.osuny.org. 

Si une modification majeure est apportée (modification du DOM, de la structure, suppression d'une classe ou d'un partiel...) il faut bien vérifier dans tous les sites que l'élément supprimé ou modifié n'est pas overridé dans un autre site. Si c'est le cas, il faut bien s'assurer de gérer la modification des cas particuliers avant de faire une release et déployer le thème sur tous les sites.

## Versionner

TODO expliquer les règles de versioning
