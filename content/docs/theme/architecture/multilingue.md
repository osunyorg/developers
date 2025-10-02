---
title: Multilingue
weight: 2
---



## Les contenus

Il y a 2 façons de gérer les contenus multilingues dans Hugo :
- toutes les langues d'un contenu dans le même répertoire
- un répertoire par langue contenant tous les contenus

Nous utilisons le second système.  
On range les contenus dans un dossier `/content/fr/`.  
Chaque fichier a son url inclue. Il faut penser à préfixer toutes les urls par la langue (`/fr/actualites/2022-01-01-mon-article`)

## Les menus

Nous n'utilisons pas les menus natifs de Hugo mais un système parallèle.  
On range les menus pour chaque langue dans un dossier `/data/menus/fr/`. Tous les target doivent être préfixés avec la langue.


## Questions / réponses sur le monolingue

**Q: Doit-on forcer le choix d'au moins une langue pour un site web ?**  
**A:** Conceptuellement un site a toujours une langue donc ça semble bien.  

**Q: Si on choisit une seule langue, doit-on écrire quand même les contenus (et les menus) dans /fr/ ?**   
**A:** Ca ne semble pas poser de problème.  

**Q: En cas de monolingue quel pattern d'url utiliser ?**   
**A:** Si une seule langue il vaut mieux que les urls ne soient pas préfixées par la langue.  
Dans ce cas les fichiers générés ne doivent pas avoir le `/:lang` dans leurs url. Idem pour les target des menus bien sûr.  
Si on est monolingue le paramètre `defaultContentLanguageInSubdir` est de toute façon ignoré par Hugo.  
En cas d'activation d'une seconde langue a posteriori le jeu d'alias des permalinks permettra de tout transférer sur /fr/  


## En résumé

### Cas site monolingue (fr)
- contenu dans `/content/fr`
- url des contenus NON préfixés par /fr (idems urls des children)
- menus dans `/menus/fr` avec des target NON préfixés par /fr
- `config/languages.yaml` contenant
```
  fr:
    title: Titre du site
    contentDir: content/fr
    languageCode: fr
    languageName: Français
```
- `config/production/config.yaml` et `config/development/config.yaml` contenant (entre autres...)
```
  ## LANGUAGE
  defaultContentLanguage: fr
  defaultContentLanguageInSubdir: false
```
(ce dernier paramètre est actuellement ignoré par Hugo si une seule langue, mais on ne sait jamais pour l'avenir...)  
- tout le site est consultable sans /fr dans les urls

### Cas site multilingue (fr / en, default fr)
- contenu FR dans /content/fr
- contenu EN dans /content/en
- url des contenus préfixés par /:lang/ (idems urls des children)
- menus FR dans /menus/fr avec des target préfixés par /fr/
- menus EN dans /menus/en avec des target préfixés par /en/
- config/languages.yaml contenant
```
  fr:
    title: Titre du site
    contentDir: content/fr
    languageCode: fr
    languageName: Français
  en:
    title: Site title
    contentDir: content/en
    languageCode: en
    languageName: English
```
- `config/production/config.yaml` et `config/development/config.yaml` contenant (entre autres...)
```
  ## LANGUAGE
  defaultContentLanguage: fr
  defaultContentLanguageInSubdir: true
```
- tout le site est consultable avec /:lang dans les urls
- la racine du site renvoie sur /fr
