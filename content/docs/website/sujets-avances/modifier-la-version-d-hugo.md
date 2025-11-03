---
title: Modifier la version de Hugo.io
weight: 1
---

## Installer une version spécifique (ex: 0.145.0)

Aller dans la liste des versions d’Hugo : https://github.com/gohugoio/hugo/releases

Aller sur la version choisie : https://github.com/gohugoio/hugo/releases/tag/v0.145.0

Dans la liste des assets, télécharger celle contenant “darwin” et toutes les options 
(extended et withdeploy) : https://github.com/gohugoio/hugo/releases/download/v0.145.0/hugo_extended_withdeploy_0.145.0_darwin-universal.tar.gz

Dézipper l’archive pour obtenir l’exécutable hugo.

Dans le Finder, faire Cmd+Shift+G, et aller dans `/opt`.

Si inexistant, créer un dossier hugo, puis à l’intérieur, un dossier bin

Déplacer l’exécutable hugo dans le dossier bin

Dans un terminal, exécuter la commande suivante pour autoriser l’exécution

`sudo xattr -d com.apple.quarantine /opt/hugo/bin/hugo`

Si c’est la première fois qu’on installe une version spécifique, éditer le fichier `.zshrc` pour mettre l’exécutable dans le `PATH`.

On édite avec la commande `nano ~/.zshrc`

Descendre tout en bas du fichier et ajouter

`export PATH=/opt/hugo/bin:$PATH`

Sauvegarder le fichier et quitter avec Ctrl+O, Entrée, Ctrl+X

Relancer un Terminal et vérifier l’installation avec la commande `hugo version`.

