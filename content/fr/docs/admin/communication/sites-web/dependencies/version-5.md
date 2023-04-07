---
title: Version 5
description: Sur les suppressions
---


## Approche 1

Dépublication objet indirect
- Pour chaque website de l'objet indirect
  - On liste les objets directs auxquels l'objet indirect est connecté
  - Pour chaque objet direct
    - On rafraîchit ses connexions et on liste les objets indirects dont la connexion devient obsolète (l'objet indirect et certaines de ses dépendances)
    - Pour chaque objet indirect (sauf l'objet indirect initial)
      - On liste les objets directs auxquels l'objet indirect est connecté, en excluant ceux de l'objet initial
      - On rafraichît les connexions de ces objets directs, car impossible de savoir si elles sont actives ou obsolètes par une ancienne dépublication
        - Si plus aucune connexion active, on note l'objet indirect comme à supprimer de git
        - Sinon on ne fait rien
  - On note l'objet indirect initial comme à supprimer de git
  - On synchronise le référentiel en supprimant de git les objets indirects listés

Exemples

T1E1 :
- Site web [W]
  - Page Person [PP]
    - Block Image [BI]
      - Image Blob [B]

Il existe donc 2 connexions via la page Person :
- W<-PP->BI
- W<-PP->B

Dépublication Block Image BI
- Pour chaque website du block (ici uniquement W via W<-PP->BI)
  - On liste les objets directs auxquels l'objet indirect est connecté
  - Pour chaque objet direct (ici uniquement PP)
    - On rafraîchit ses connexions et on liste les objets indirects dont la connexion devient obsolète (l'objet indirect et certaines de ses dépendances)
    - Pour chaque objet indirect (sauf l'objet indirect initial) (ici uniquement B)
      - On liste les objets directs auxquels l'objet indirect est connecté, en excluant ceux de l'objet initial (ici rien)
      - *étape sautée* : On rafraichît les connexions de ces objets directs, car impossible de savoir si elles sont actives ou obsolètes par une ancienne dépublication
        - Si plus aucune connexion active, on note l'objet indirect comme à supprimer de git (ici on supprime B)
        - *étape sautée* : Sinon on ne fait rien
  - On note l'objet indirect initial comme à supprimer de git (ici on supprime BI)
  - On synchronise le référentiel en supprimant de git les objets indirects listés (on supprime BI et B)

T1E2 :
- Site web [W]
  - Page Accueil [HP]
    - Block Person [BP]
      - Olivia [OS]
        - Avatar Blob [B]
  - Page Person [PP]
    - Olivia [OS]
      - Avatar Blob [B]
      

Il existe donc 5 connexions :
- 3 via la page Accueil
  - W<-HP->BP
  - W<-HP->OS
  - W<-HP->B
- 2 via la page Equipe
  - W<-PP->OS
  - W<-PP->B

Dépublication Block Person BP
- Pour chaque website du block (ici uniquement W via W<-HP->BP)
  - On liste les objets directs auxquels l'objet indirect est connecté
  - Pour chaque objet direct (ici uniquement HP)
    - On rafraîchit ses connexions et on liste les objets indirects dont la connexion devient obsolète (l'objet indirect et certaines de ses dépendances)
    - Pour chaque objet indirect (sauf l'objet indirect initial) (ici OS et B)
      - On liste les objets directs auxquels l'objet indirect est connecté, en excluant ceux de l'objet initial (ici PP)
      - On rafraichît les connexions de ces objets directs, car impossible de savoir si elles sont actives ou obsolètes par une ancienne dépublication
        - *étape sautée* : Si plus aucune connexion active, on note l'objet indirect comme à supprimer de git
        - Sinon on ne fait rien (ici on conserve OS et B car leurs connexions via PP sont toujours actives)
  - On note l'objet indirect initial comme à supprimer de git (ici on supprime BP)
  - On synchronise le référentiel en supprimant de git les objets indirects listés (on supprime BP)

T1E3 :
- Site web [W]
  - Page Person [PP]
    - Olivia [OS]
        - Avatar Blob [OSB]
    - Pierre-André [PA]
        - Avatar Blob [PAB]
        - Block Person [BP]
          - Olivia [OS]
            - Avatar Blob [OSB]

Il existe donc 5 connexions via la page Person :
  - W<-PP->OS
  - W<-PP->OSB
  - W<-PP->PA

Dépublication Block Person BP
- Pour chaque website du block (ici uniquement W via W<-HP->BP)
  - On liste les objets directs auxquels l'objet indirect est connecté
  - Pour chaque objet direct (ici uniquement HP)
    - On rafraîchit ses connexions et on liste les objets indirects dont la connexion devient obsolète (l'objet indirect et certaines de ses dépendances)
    - Pour chaque objet indirect (sauf l'objet indirect initial) (ici OS et B)
      - On liste les objets directs auxquels l'objet indirect est connecté, en excluant ceux de l'objet initial (ici PP)
      - On rafraichît les connexions de ces objets directs, car impossible de savoir si elles sont actives ou obsolètes par une ancienne dépublication
        - *étape sautée* : Si plus aucune connexion active, on note l'objet indirect comme à supprimer de git
        - Sinon on ne fait rien (ici on conserve OS et B car leurs connexions via PP sont toujours actives)
  - On note l'objet indirect initial comme à supprimer de git (ici on supprime BP)
  - On synchronise le référentiel en supprimant de git les objets indirects listés (on supprime BP)


T1E4 :
- Site web [W]
  - Page Accueil [HP]
    - Block Person [BP]
      - Olivia [OS]
        - Avatar Blob [B]
    - Block Person [BP2]
      - Pierre-André [PA]
        - Block Person [BP3]
          -  Olivia [OS]
            - Avatar Blob [B]


## Approche 2

Dépublication objet indirect
- Pour chaque website de l'objet indirect
  - On liste les objets directs auxquels l'objet indirect est connecté
  - Pour chaque objet direct
    - On liste les dépendances récursives de cet objet, en ignorant les objets non synchronisables (et leurs dépendances).
    - 
      - On liste les objets directs auxquels l'objet indirect est connecté, en excluant ceux de l'objet initial
      - On rafraichît les connexions de ces objets directs, car impossible de savoir si elles sont actives ou obsolètes par une ancienne dépublication
        - Si plus aucune connexion active, on note l'objet indirect comme à supprimer de git
        - Sinon on ne fait rien
  - On note l'objet indirect initial comme à supprimer de git
  - On synchronise le référentiel en supprimant de git les objets indirects listés

T2E1

- Site web [W]
  - Page Accueil [HP]
    - Block Person [BP]
      - Arnaud [AL]
      - Olivia [OS]
        - Avatar Blob [B]
    - Block Person [BP2]
      - Pierre-André [PA]
        - Block Person [BP3]
          -  Olivia [OS]
            - Avatar Blob [B]
  - Page test [PT]
    - Block Person [BP4]
      - Arnaud [AL]

## Approche 3

Dépublication objet indirect
- Pour chaque website de l'objet indirect
  - On liste les dépendances récursives du website, en ignorant les objets non synchronisables (et leurs dépendances).
  - On liste les dépendances récursives du website, avec les objets non synchronisables
  - On soustrait pour récupérer les dépendances qui ne sont plus synchronisables
  - On supprime ces dépendances du référentiel Git


- Olivia est connectée via Pierre-André (person.blocks) qui est connecté via une formation (program.blocks)
- Olivia est aussi connectée via noesya (organization.blocks) qui est connectée via une formation (program.blocks)
- Pierre-André est dépublié, tandis que noesya est publiée

- Site web [W]
  - Page Accueil [HP]
    - Block Person [BP] (dépublié)
      - Arnaud [AL]
      - Olivia [OS]
        - Avatar Blob [B]
    - Block Person [BP2]
      - Pierre-André [PA]
        - Block Person [BP3]
          -  Olivia [OS]
            - Avatar Blob [B]

- On liste les dépendances récursives du website, en ignorant les objets non synchronisables (et leurs dépendances).

HP, BP2, PA, BP3, OS, B
HP, BP, AL, OS, B, BP2, PA, BP3

AL

Cette approche revient à trouver ce qui n'est plus dans la liste, à faire une soustraction entre 2 listes.
Si on peut le faire au niveau du site, cela permet de déclencher la suppression du git des objets qui ne sont plus là.
Cette même approche au niveau d'un objet, direct ou indirect, permet de vérifier s'il y a eu suppression.
S'il n'y en a pas (rien à enlever), alors on peut gérer les dépendances locales. 
S'il y en a (des choses à enlever), alors il faut lancer le nettoyage du site.
Le même algorithme devrait servir pour les 2 cas, et doit être particulièrement optimisé en mémoire vive.

## Approche 4

*Work in progress*

Dépublication Block Person BP
- Pour chaque website de l'objet indirect (uniquement W)
  - On liste les objets directs auxquels l'objet indirect est connecté (uniquement HP)
  - Pour chaque objet direct (uniquement HP)
    - On liste les dépendances récursives de cet objet, avec les objets non synchronisables (et leurs dépendances).
      - On aura : BP, AL, OS, B, BP2, PA, BP3
    - On liste les dépendances récursives de cet objet, en ignorant les objets non synchronisables (et leurs dépendances).
      - On aura : BP2, PA, BP3, OS, B
    - On soustrait pour récupérer les dépendances qui ne sont plus synchronisables
      - On aura : BP, AL
    - ?
