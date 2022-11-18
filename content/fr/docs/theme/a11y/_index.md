---
title: Accessibilité
weight: 3
description: >
  Permettre à chaque personne d'utiliser le site Web
---

L'objectif est de rendre les sites produits avec Osuny les plus accessibles possibles par défaut. 
Deux problématiques s'entrelacent, la qualité technique du balisage HTML et la qualité du contenu produit par les contribut·eur·rice·s.
Plusieurs critères du [RGAA](https://accessibilite.numerique.gouv.fr/methode/criteres-et-tests/) sont cependant encore sujets à questionnement.

## Balises alt

Nous nous sommes longuement demandé comment savoir si la présence d’un texte alternatif décrivant une image est nécessaire. Pour cela, il faut déterminer si cette image est purement [décorative](https://accessibilite.numerique.gouv.fr/methode/glossaire/#image-de-decoration) ou [porteuse d’information](https://accessibilite.numerique.gouv.fr/methode/glossaire/#image-porteuse-d-information).

Cette décision est à la charge de l’éditeur·ice du site, bien qu'une indication lui permet de se souvenir qu'il faut parfois un `alt` et parfois pas.

### Quand utiliser un alt ?

L’enjeu est de savoir à quel type l’image correspond. Un questionnement court et simple permet de le déterminer et dans l’idéal, Osuny pourrait proposer un algorithme aidant l’éditeur·ice à répondre à cette question, en cas de doute. Cet algorithme prendrait la forme d'une série de questions avec une préconisation et des exemples.

L’objectif serait ainsi de réaliser une sorte d’*alt builder* dans le back office, sur le modèle de l’« [l’arbre de décision relatif à l’alt](https://www.w3.org/WAI/tutorials/images/decision-tree/) » du W3C, posant quelques questions et générant au besoin un `alt` adéquat.

Pour confirmer nos hypothèses, un travail en coopération avec des utilisatrices et utilisateurs concerné·e·s par les enjeux de l'accessibilité est nécessaire.

### Comment rédiger un alt ?

Il ne faut pas faire figurer dans un `alt` : 
* « Photo de [...] » ;
* « Image de [...] » ;

En effet, un lecteur d’écran indique automatiquement la présence d’une image. Un texte alternatif avec ces mots serait donc lu de cette façon : «  Image. Image de [...] ».

La rédaction d’un texte alternatif pour une image doit obéir à certains critères favorisant l’expérience des utilisateur·ice·s :
* ne pas dépasser 100 caractères ;
* correspondre au ton et à la rédaction du contenu que l’image vient enrichir : il peut par exemple faire figurer des détails et faire passer des émotions, tant que cela reste cohérent avec le contenu auquel il se réfère.

## Multimedia

Les vidéos posent de nombreuses questions, auxquelles Osuny répond en grande partie. Néanmoins, l’objectif est d'arriver à un taux de conformité maximal sans dérogation.

Pour cela, nous avons pris des dispositions susceptibles d’être améliorées ou enrichies :
* Permettre l’ajout d’une **transcription vidéo** ([critère 4.1](https://accessibilite.numerique.gouv.fr/methode/criteres-et-tests/#4.1)) ;
* Indiquer aux utilisateur·ice·s la **présence d’un contenu multimédia** ([critère 4.7](https://accessibilite.numerique.gouv.fr/methode/criteres-et-tests/#4.7)) : en ajoutant un titre au bloc et/ou en employant l’attribut [aria-description](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Attributes/aria-description) ;
* Indiquer aux utilisateur·ice·s la **présence ou non de sous-titres**, au moyen d’un bouton dans le back office d’Osuny.

## Audit RGAA

Un premier audit des sites réalisés avec Osuny a été réalisé à l’été 2022, avec pour résultat :
* **95.9%** des critères RGAA respectés ;
* **98.8%** de conformité moyenne du service en ligne.

Un [nouvel audit](https://ara.numerique.gouv.fr/rapports/U4SSpScTL2OP0tGrt-tt6) (réalisé sur le site de l’[IUT Bordeaux Montaigne](https://www.iut.u-bordeaux-montaigne.fr/)), en date du 17 novembre 2022, donnait un résultat légèrement différent, du fait des nouvelles pages auditées, avec un taux global de conformité au RGAA de **93%**.

**NB :** l’[ARA](https://ara.numerique.gouv.fr/) ne proposant pas de déroger aux critères, certains critères ne pouvant s’appliquer ont été renseignés comme « non conformes ».

Qui plus est, cet audit a mis en avant de nouvelles questions auxquelles Osuny apportera prochainement des réponses, ce qui permettrait d’augmenter encore le score de conformité global du site.

## Déclaration d'accessibilité

### Dérogations

#### Héritages et outils externes

Les sites réalisés avec Osuny pouvant être issus de migrations, certains contenus issus des archives des anciens sites peuvent ne pas être conformes au RGAA.

L'outil [Summernote](https://summernote.org/) utilisé pour l'édition HTML ne permet pas par défaut de gérer les titres des liens, ce qui cause le non-respect du [critère 6.1.1](https://accessibilite.numerique.gouv.fr/methode/criteres-et-tests/#6.1).

#### Multimédia : contenus non soumis à l'obligation d'accessibilité

> [Critère 4.3](https://accessibilite.numerique.gouv.fr/methode/criteres-et-tests/#4.3) : Chaque média temporel synchronisé pré-enregistré a-t-il, si nécessaire, des sous-titres synchronisés (hors cas particuliers) ?

> [Critère 4.5](https://accessibilite.numerique.gouv.fr/methode/criteres-et-tests/#4.5) : Chaque média temporel pré-enregistré a-t-il, si nécessaire, une audiodescription synchronisée (hors cas particuliers) ?

Les contenus multimédia (vidéos) ne peuvent être pris en compte dans les audits d’accessibilité, d’une part parce que cela représenterait une charge conséquente de travail pour les éditeur·ice·s du site, d’autre part parce qu’il peut leur être impossible d’avoir la main sur les vidéos, pouvant provenir d’une chaîne extérieure.

En revanche, il est possible de tout mettre en œuvre pour améliorer l’accessibilité de ces contenus, en indiquant aux utilisateurs la présence ou non de sous-titres ou d’audiodescription synchronisée.