---
title: "i18n"
---

## Traduction du thème

Le thème est traduit en anglais et portugais. Les fichiers se trouvent dans `osuny/i18n/`.

Vous pouvez modifier les clés en override en ajoutant un dossier dans votre site `/i18n/`. Hugo fusionne les clés, vous pouvez alors ajouter ou modifier uniquement les clés souhaitées (sans copier-coller tout le fichier de traduction).

## Recherche d'une préférence de langue et redirection automatique

On redirige automatiquement les utilisateurices vers la langue de leur navigateur si le site est traduit dans cette même langue.

Dans le fichier `alias.html` :

- on récupère la langue préférée du navigateur
- on vérifie qu'on est sur la page d'accueil
- on cherche une correspondance dans les langues disponibles du site avec la langue du navigateur
- si il y a une correspondance, on redirige sur la page d'accueil dans la bonne langue
- sinon, on redirige sur la langue par défaut (la master du site)

Merci à nanmu42 ! https://nanmu.me/en/posts/2020/hugo-i18n-automatic-language-redirection/


