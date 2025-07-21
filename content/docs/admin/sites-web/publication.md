---
title: Publication
---

## Situation au 21 juillet 2025

### syncable?
l'objet doit-il (globalement) aller sur Git  
=> renommé `can_have_git_file?`

### exportable_to_git?
localization du website non exportable ? + category exportable ?  
=> renommé `should_sync_to?(website)`

### published_in?(website)
logique algo (ex: est-ce que l'événement est autorisé dans le website)  
=> fusionné dans la nouvelle `should_sync_to?(website)`

### for_website? 
autre logique algo (ex: est-ce que l'événement est autorisé dans le website) 
=> fusionné dans la nouvelle `should_sync_to?(website)`

### path is empty 
logique applicative (les early return si pas published? par exemple). 
=> supprimé. Le path est maintenant toujours rempli, même si la page n'est plus publiée.

### is_necessary_for_website? 
uniquement pour les pages spéciales. Conditionne leur création.  
=> renommé `should_create_special_page?`

## Cas du `syncable_only`

On gère le cas du block :
- qui n'a pas de git file
- qui peut être publié ou pas
- et qui a des dépendances.


Concrètement : 
- si le bloc est publié, il faut suivre ses dépendances
- si le bloc n'est pas publié, il faut s'arrêter là


Dans le calcul des dépendances, on se fiche de la présence ou de l'absence de git_file, on veut juste parcourir récursivement les objets qui doivent être envoyés sur le site.
Il faut utiliser `published?` pour vérifier si on creuse encore.
Le website, le menu ne répondent pas à `published?`

On essaie là d'enlever tous les appels aux méthodes de synchronisation. La liste de dépendances ne doit pas mélanger le syncable. Par contre on doit vérifier si les objets sont publiés.


## Nouvelle structure simplifiée

### can_have_git_file?
Renvoie un simple booléen (ou n'existe pas).  
Soit l'objet ne l'implémente pas, et le `try(:can_have_git_file?)` renvoie false.  
Soit l'objet l'implémente spécifiquement, comme les Blobs.  
Soit l'objet override la méthode pour indiquer une absence, comme les Alumni.

### should_sync_to?(website) 
Méthode avec logique contextuelle

### should_create_special_page? 
Uniquement pour les pages spéciales. Conditionne leur création