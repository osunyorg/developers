---
title: Modifier la version de Hugo
weight: 1
date: 2026-01-26
---

## Sur Mac

### Avec Homebrew

1. Aller chercher le commit sur [Github](https://github.com/search?q=repo%3AHomebrew%2Fhomebrew-core+hugo&type=commits)
2. Chercher la bonne version (ex: [hugo : update 0.95.0 bottle](https://github.com/Homebrew/homebrew-core/commit/c27b162abd2f40785678da93924b47660bf98f5a))
3. Dans le menu `...`, choisir `View file`
4. Cliquer le bouton `Raw`
5. Télécharger le fichier (n'importe où)
6. Lancer `HOMEBREW_DEVELOPER=true brew install hugo.rb` (dans le même répertoire)

Versions intéressantes
- [Hugo 0.154.5](https://raw.githubusercontent.com/Homebrew/homebrew-core/7a8acec609d49396c95c76e7b294fa85f0bb1318/Formula/h/hugo.rb)

Sources : 
- [Switching Hugo versions](https://discourse.gohugo.io/t/switching-hugo-versions/38251/2)
- [How to downgrade hugo to a previous version?](https://discourse.gohugo.io/t/how-to-downgrade-hugo-to-a-previous-version/40388/5)

### Sans outil

#### Après la 0.153.0

Aller dans la liste des versions d’Hugo : https://github.com/gohugoio/hugo/releases

Aller sur la version choisie : https://github.com/gohugoio/hugo/releases/tag/v0.154.5

Dans la liste des assets, télécharger celle contenant “darwin” et toutes les options (extended et withdeploy) : hugo_extended_withdeploy_0.154.5_darwin-universal.pkg

Dans un Terminal, aller dans le dossier du téléchargement, puis décompresser le pkg avec la commande suivante

`pkgutil --expand-full hugo_0.154.5_darwin-universal.pkg hugo_0.154.5`

Ouvrir le Finder dans le dossier contenant le bin avec la commande suivante

`open hugo_0.154.5/Payload`

Ouvrez une autre fenêtre de Finder, faire `Cmd+Shift+G`, et aller dans `/opt`.

Si inexistant, créer un dossier hugo, puis à l’intérieur, un dossier `bin`

Déplacer l’exécutable hugo du dossier Payload vers ce dossier `bin`

Si c’est la première fois qu’on installe une version spécifique, éditer le fichier `.zshrc` pour mettre l’exécutable dans le `PATH`.

On édite avec la commande `nano ~/.zshrc`

Descendre tout en bas du fichier et ajouter

export `PATH=/opt/hugo/bin:$PATH`

Sauvegarder le fichier et quitter avec `Ctrl+O`, `Entrée`, `Ctrl+X`

Relancer un Terminal et vérifier l’installation avec `hugo version`

## Avant la 0.153.0

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

