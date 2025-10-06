---
title: Soft release
---

## Problématique

Cela prend beaucoup de temps de vérifier les sites un par un avec l'outil backstop js.

## Cas concret

Pour des releases mineur ou majeur, une étape de vérification semi-manuelle permet de s'assurer qu'il n'y ai pas de regression visuelle.

Pour chaque site, on l'ouvre en local, et on lance la commande `osuny backstop` pour vérifier des diffs local / production. Il faut attendre la fin du process pour vérifier le site, puis lancer l'analyse du suivant.

## Situation idéale

On lance une commande bulk sur plusieurs sites et on obtient un rapport final sur l'ensemble des sites testé.

## Solution proposée

Sur une machine locale, on lance la commande avec une liste de sites. Une fois le processus terminé on a un retour sur l'ensemble des différences des sites.