---
title: Audit de site
weight: 2
aliases:
  - /docs/theme/a11y/change-me/
---

*Réaliser l'audit RGAA d'un site créé avec Osuny, en partant de l'audit socle du thème*

## Légendes 

> [!CAUTION] Tester normalement
> Cela signifie que le thème Osuny ne résout pas ce problème nativement, donc il faut tester le critère.

> [!WARNING] Vérifier les surcouches HTML et CSS
> Cela signifie que le thème Osuny résout le problème, et qu'il faut juste vérifier que l'HTML et le CSS spécifique du site ne cause pas de régression.

> [!TIP] Pas besoin de tester
> Cela signifie qu'il n'y a pas besoin de tester ce critère, tout est déjà géré avec le thème.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

## 1. Images 

### 1.1 🔴
Chaque image porteuse d’information a-t-elle une alternative textuelle ?
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 1.2 🟠
Chaque image de décoration est-elle correctement ignorée par les technologies d’assistance ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Il n'est pas possible d'envoyer des images de décoration via l'admin.
> La seule possibilité de casser ce critère est dans les surcouches HTML.

### 1.3 🔴
Pour chaque image porteuse d’information ayant une alternative textuelle, cette alternative est-elle pertinente (hors cas particuliers) ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 1.4 🟢
Pour chaque image utilisée comme CAPTCHA ou comme image-test, ayant une alternative textuelle, cette alternative permet-elle d’identifier la nature et la fonction de l’image ? 
> [!TIP] Pas besoin de tester
> Vérifier l'absence de CAPTCHA dans les blocs d'intégration HTML (embed).
> Comme il n'y a pas de captcha dans le thème Osuny, s'il n'y en a pas dans les blocs embed, le critère est non applicable.

### 1.5 🟢
Pour chaque image utilisée comme CAPTCHA, une solution d’accès alternatif au contenu ou à la fonction du CAPTCHA est-elle présente ? 
> [!TIP] Pas besoin de tester
> Vérifier l'absence de CAPTCHA dans les blocs d'intégration HTML (embed).
> Comme il n'y a pas de captcha dans le thème Osuny, s'il n'y en a pas dans les blocs embed, le critère est non applicable.

### 1.6 🔴
Chaque image porteuse d’information a-t-elle, si nécessaire, une description détaillée ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 1.7 🔴
Pour chaque image porteuse d’information ayant une description détaillée, cette description est-elle pertinente ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 1.8 🔴
Chaque image texte porteuse d’information, en l’absence d’un mécanisme de remplacement, doit si possible être remplacée par du texte stylé. Cette règle est-elle respectée (hors cas particuliers) ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 1.9 🔴
Chaque légende d’image est-elle, si nécessaire, correctement reliée à l’image correspondante ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution, on peut remplir les légendes à des endroits incorrects en admin.

## 2. Cadres 

### 2.1 🔴
Chaque cadre a-t-il un titre de cadre ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 2.2 🔴
Pour chaque cadre ayant un titre de cadre, ce titre de cadre est-il pertinent ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

## 3. Couleurs 

### 3.1 🟠
Dans chaque page web, l’information ne doit pas être donnée uniquement par la couleur. Cette règle est-elle respectée ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Le thème Osuny ne donne jamais d'information uniquement par la couleur.

### 3.2 🟠
Dans chaque page web, le contraste entre la couleur du texte et la couleur de son arrière-plan est-il suffisamment élevé (hors cas particuliers) ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Tous les taux de contraste du thème Osuny sont respectueux du WCAG AAA.

### 3.3 🟠
Dans chaque page web, les couleurs utilisées dans les composants d’interface ou les éléments graphiques porteurs d’informations sont-elles suffisamment contrastées (hors cas particuliers) ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Tous les taux de contraste du thème Osuny sont respectueux du WCAG AAA.

## 4. Multimédia 

### 4.1 🔴
Chaque média temporel pré-enregistré a-t-il, si nécessaire, une transcription textuelle ou une audiodescription (hors cas particuliers) ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 4.2 🔴
Pour chaque média temporel pré-enregistré ayant une transcription textuelle ou une audiodescription synchronisée, celles-ci sont-elles pertinentes (hors cas particuliers) ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 4.3 🔴
Chaque média temporel synchronisé pré-enregistré a-t-il, si nécessaire, des sous-titres synchronisés (hors cas particuliers) ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 4.4 🔴
Pour chaque média temporel synchronisé pré-enregistré ayant des sous-titres synchronisés, ces sous-titres sont-ils pertinents ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 4.5 🔴
Chaque média temporel pré-enregistré a-t-il, si nécessaire, une audiodescription synchronisée (hors cas particuliers) ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 4.6 🔴
Pour chaque média temporel pré-enregistré ayant une audiodescription synchronisée, celle-ci est-elle pertinente ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 4.7 🟠
Chaque média temporel est-il clairement identifiable (hors cas particuliers) ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Les médias temporels sont signalés par des lecteurs.

### 4.8 🔴
Chaque média non temporel a-t-il, si nécessaire, une alternative (hors cas particuliers) ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 4.9 🔴
Pour chaque média non temporel ayant une alternative, cette alternative est-elle pertinente ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 4.10 🟠
Chaque son déclenché automatiquement est-il contrôlable par l’utilisateur ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Le thème Osuny ne force aucun son.

### 4.11 🟠
La consultation de chaque média temporel est-elle, si nécessaire, contrôlable par le clavier et tout dispositif de pointage ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Le thème Osuny utilise le lecteur natif pour le son, et délègue la vidéo aux plateformes choisies en contribution (Vimeo, Youtube...).

### 4.12 🟠
La consultation de chaque média non temporel est-elle contrôlable par le clavier et tout dispositif de pointage ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Le thème Osuny est entièrement navigable au clavier, avec une attention particulière aux "focus traps".

### 4.13 🔴
Chaque média temporel et non temporel est-il compatible avec les technologies d’assistance (hors cas particuliers) ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

## 5. Tableaux

### 5.1 🔴
Chaque tableau de données complexe a-t-il un résumé ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 5.2 🔴
Pour chaque tableau de données complexe ayant un résumé, celui-ci est-il pertinent ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 5.3 🔴
Pour chaque tableau de mise en forme, le contenu linéarisé reste-t-il compréhensible ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 5.4 🔴
Pour chaque tableau de données ayant un titre, le titre est-il correctement associé au tableau de données ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 5.5 🔴
Pour chaque tableau de données ayant un titre, celui-ci est-il pertinent ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 5.6 🟠
Pour chaque tableau de données, chaque en-tête de colonne et chaque en-tête de ligne sont-ils correctement déclarés ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Le thème Osuny balise correctement les tableaux avec le bloc "Tableau de données".

### 5.7 🟠
Pour chaque tableau de données, la technique appropriée permettant d’associer chaque cellule avec ses en-têtes est-elle utilisée (hors cas particuliers) ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Le thème Osuny balise correctement les tableaux avec le bloc "Tableau de données".

### 5.8 🟠
Chaque tableau de mise en forme ne doit pas utiliser d’éléments propres aux tableaux de données. Cette règle est-elle respectée ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Il n'y a pas de tableau de mise en forme dans le thème Osuny.
> La seule possibilité de casser ce critère est dans les surcouches HTML.

## 6. Liens 

### 6.1 🔴
Chaque lien est-il explicite (hors cas particuliers) ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 6.2 🟢
Dans chaque page web, chaque lien a-t-il un intitulé ? 
> [!TIP] Pas besoin de tester
> Tous les liens du thème ont un intitulé, qu'il s'agisse de liens structurels ou des liens à l'intérieur des textes contribués.

## 7. Scripts 

### 7.1 🟢
Chaque script est-il, si nécessaire, compatible avec les technologies d’assistance ? 
> [!TIP] Pas besoin de tester
> Tous les scripts du thème sont sains.

### 7.2 🟢
Pour chaque script ayant une alternative, cette alternative est-elle pertinente ? 
> [!TIP] Pas besoin de tester
> Le thème fournit un socle sain.

### 7.3 🟢
Chaque script est-il contrôlable par le clavier et par tout dispositif de pointage (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Le thème fournit un socle sain.

### 7.4 🟢
Pour chaque script qui initie un changement de contexte, l’utilisateur est-il averti ou en a-t-il le contrôle ? 
> [!TIP] Pas besoin de tester
> Le thème fournit un socle sain.

### 7.5 🟢
Dans chaque page web, les messages de statut sont-ils correctement restitués par les technologies d’assistance ? 
> [!TIP] Pas besoin de tester
> Le thème fournit un socle sain.

## 8. Éléments obligatoires 

### 8.1 🟢
Chaque page web est-elle définie par un type de document ? 
> [!TIP] Pas besoin de tester
> Le thème spécifie le doctype.

### 8.2 🟢
Pour chaque page web, le code source généré est-il valide selon le type de document spécifié ? 
> [!TIP] Pas besoin de tester
> Le thème est en surveillance continue sur https://w3c.noesya.coop.

### 8.3 🟢
Dans chaque page web, la langue par défaut est-elle présente ? 
> [!TIP] Pas besoin de tester
> La langue est cohérente avec la contribution en admin.

### 8.4 🟢
Pour chaque page web ayant une langue par défaut, le code de langue est-il pertinent ? 
> [!TIP] Pas besoin de tester
> Les codes de langues utilisés par Osuny respectent la spécification ISO 639-1.

### 8.5 🟢
Chaque page web a-t-elle un titre de page ? 
> [!TIP] Pas besoin de tester
> Les titres sont obligatoires en admin.

### 8.6 🔴
Pour chaque page web ayant un titre de page, ce titre est-il pertinent ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 8.7 🔴
Dans chaque page web, chaque changement de langue est-il indiqué dans le code source (hors cas particuliers) ? 
> [!CAUTION] Tester normalement
> Osuny n'est pas capable de gérer les extraits de texte dans une langue différente de la page.

### 8.8 🔴
Dans chaque page web, le code de langue de chaque changement de langue est-il valide et pertinent ? 
> [!CAUTION] Tester normalement
> Osuny n'est pas capable de gérer les extraits de texte dans une langue différente de la page.

### 8.9 🟢
Dans chaque page web, les balises ne doivent pas être utilisées uniquement à des fins de présentation. Cette règle est-elle respectée ? 
> [!TIP] Pas besoin de tester
> Osuny supprime les attributs et balises de présentation envoyées en contribution, et n'utilise pas de balise à des fins de présentation dans le thème.

### 8.10 🔴
Dans chaque page web, les changements du sens de lecture sont-ils signalés ? 
> [!CAUTION] Tester normalement
> Osuny ne gère pas encore le RTL.

## 9. Structuration de l’information 

### 9.1 🔴
Dans chaque page web, l’information est-elle structurée par l’utilisation appropriée de titres ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 9.2 🟢
Dans chaque page web, la structure du document est-elle cohérente (hors cas particuliers) ?
> [!TIP] Pas besoin de tester
> Osuny gère automatiquement les titres.

### 9.3 🟢
Dans chaque page web, chaque liste est-elle correctement structurée ? 
> [!TIP] Pas besoin de tester
> Osuny gère automatiquement l'arborescence de titres, cf l'article [Accessibilité numérique : un bon plan](https://lab.noesya.coop/publications/2023-12-07-accessibilite-numerique-un-bon-plan/).

### 9.4 🔴
Dans chaque page web, chaque citation est-elle correctement indiquée ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.
> Si les citations utilisent les blocs "Témoignages", le balisage est nativement correct.

## 10. Présentation de l’information 

### 10.1 🟢
Dans le site web, des feuilles de styles sont-elles utilisées pour contrôler la présentation de l’information ? 
> [!TIP] Pas besoin de tester
> Osuny fournit les feuilles de style.

### 10.2 🟢
Dans chaque page web, le contenu visible porteur d’information reste-t-il présent lorsque les feuilles de styles sont désactivées ? 
> [!TIP] Pas besoin de tester
> Osuny fournit un contenu correct sans style.

### 10.3 🟢
Dans chaque page web, l’information reste-t-elle compréhensible lorsque les feuilles de styles sont désactivées ? 
> [!TIP] Pas besoin de tester
> Osuny fournit un contenu correct sans style.

### 10.4 🟢
Dans chaque page web, le texte reste-t-il lisible lorsque la taille des caractères est augmentée jusqu’à 200 %, au moins (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Osuny fournit un contenu correct sans style.

### 10.5 🟢
Dans chaque page web, les déclarations CSS de couleurs de fond d’élément et de police sont-elles correctement utilisées ? 
> [!TIP] Pas besoin de tester
> Le CSS d'Osuny utilise correctement les couleurs de fond et de police.

### 10.6 🟢
Dans chaque page web, chaque lien dont la nature n’est pas évidente est-il visible par rapport au texte environnant ? 
> [!TIP] Pas besoin de tester
> Le CSS d'Osuny met en valeur les liens.

### 10.7 🟢
Dans chaque page web, pour chaque élément recevant le focus, la prise de focus est-elle visible ? 
> [!TIP] Pas besoin de tester
> Le CSS d'Osuny ne masque pas le focus.

### 10.8 🟢
Pour chaque page web, les contenus cachés ont-ils vocation à être ignorés par les technologies d’assistance ? 
> [!TIP] Pas besoin de tester
> Les contenus cachés sont ignorés.

### 10.9 🟠
Dans chaque page web, l’information ne doit pas être donnée uniquement par la forme, taille ou position. Cette règle est-elle respectée ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Le thème Osuny fournit des informations textuelles pour tout, il faut vérifier que cela n'est pas endommagé par les surcouches.

### 10.10 🟠
Dans chaque page web, l’information ne doit pas être donnée par la forme, taille ou position uniquement. Cette règle est-elle implémentée de façon pertinente ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Le thème Osuny fournit des informations textuelles pour tout, il faut vérifier que cela n'est pas endommagé par les surcouches

### 10.11 🟠
Pour chaque page web, les contenus peuvent-ils être présentés sans perte d’information ou de fonctionnalité et sans avoir recours soit à un défilement vertical pour une fenêtre ayant une hauteur de 256 px, soit à un défilement horizontal pour une fenêtre ayant une largeur de 320 px (hors cas particuliers) ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Le thème Osuny gère correctement les contenus trop larges.

### 10.12 🟠
Dans chaque page web, les propriétés d’espacement du texte peuvent-elles être redéfinies par l’utilisateur sans perte de contenu ou de fonctionnalité (hors cas particuliers) ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Le thème Osuny gère correctement l'espacement du texte.

Pour faciliter ce test, exécutez ce code directement dans la console js du navigateur :

```js
var style = document.createElement('style');
style.innerHTML =
  `
  * { 
    line-height: 1.5em !important;
    word-spacing: 0.16em !important;
    letter-spacing: 0.12em !important;
  }
  p {
    margin-bottom: 2em !important;
  }
  `;
document.body.append(style);
```

### 10.13 🟠
Dans chaque page web, les contenus additionnels apparaissant à la prise de focus ou au survol d’un composant d’interface sont-ils contrôlables par l’utilisateur (hors cas particuliers) ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Le thème Osuny gère correctement le focus.

### 10.14 🟠
Dans chaque page web, les contenus additionnels apparaissant via les styles CSS uniquement peuvent-ils être rendus visibles au clavier et par tout dispositif de pointage ? 
> [!WARNING] Vérifier les surcouches HTML et CSS
> Le thème Osuny gère correctement les contenus additionnels.

## 11. Formulaires 

### 11.1 🟢
Chaque champ de formulaire a-t-il une étiquette ?
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

### 11.2 🟢
Chaque étiquette associée à un champ de formulaire est-elle pertinente (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

### 11.3 🟢
Dans chaque formulaire, chaque étiquette associée à un champ de formulaire ayant la même fonction et répétée plusieurs fois dans une même page ou dans un ensemble de pages est-elle cohérente ? 
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

### 11.4 🟢
Dans chaque formulaire, chaque étiquette de champ et son champ associé sont-ils accolés (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

### 11.5 🟢
Dans chaque formulaire, les champs de même nature sont-ils regroupés, si nécessaire ? 
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

### 11.6 🟢
Dans chaque formulaire, chaque regroupement de champs de même nature a-t-il une légende ? 
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

### 11.7 🟢
Dans chaque formulaire, chaque légende associée à un regroupement de champs de même nature est-elle pertinente ? 
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

### 11.8 🟢
Dans chaque formulaire, les items de même nature d’une liste de choix sont-ils regroupés de manière pertinente ? 
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

### 11.9 🟢
Dans chaque formulaire, l’intitulé de chaque bouton est-il pertinent (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

### 11.10 🟢
Dans chaque formulaire, le contrôle de saisie est-il utilisé de manière pertinente (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

### 11.11 🟢
Dans chaque formulaire, le contrôle de saisie est-il accompagné, si nécessaire, de suggestions facilitant la correction des erreurs de saisie ? 
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

### 11.12 🟢
Pour chaque formulaire qui modifie ou supprime des données, ou qui transmet des réponses à un test ou à un examen, ou dont la validation a des conséquences financières ou juridiques, les données saisies peuvent-elles être modifiées, mises à jour ou récupérées par l’utilisateur ? 
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

### 11.13 🟢
La finalité d’un champ de saisie peut-elle être déduite pour faciliter le remplissage automatique des champs avec les données de l’utilisateur ? 
> [!TIP] Pas besoin de tester
> Il n'y a qu'un formulaire natif dans Osuny, la recherche, et il respecte tous les critères 11.
> Il faut toujours vérifier les blocs d'intégration HTML, qui ajoutent du code totalement étranger à Osuny.

## 12. Navigation

### 12.1 🟢
Chaque ensemble de pages dispose-t-il de deux systèmes de navigation différents, au moins (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Le thème Osuny est entièrement navigable à la souriss, en tactile, au clavier et en vocal.

### 12.2 🟢
Dans chaque ensemble de pages, le menu et les barres de navigation sont-ils toujours à la même place (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Le thème Osuny fixe la position des menus.

### 12.3 🟢
La page « plan du site » est-elle pertinente ? 
> [!TIP] Pas besoin de tester
> Le thème Osuny automatise le plan de site.

### 12.4 🟢
Dans chaque ensemble de pages, la page « plan du site » est-elle accessible à partir d’une fonctionnalité identique ? 
> [!TIP] Pas besoin de tester
> Le plan de site est accessible en pied de chaque page.

### 12.5 🟢
Dans chaque ensemble de pages, le moteur de recherche est-il atteignable de manière identique ? 
> [!TIP] Pas besoin de tester
> La recherche, si elle est activée, est présente sur toutes les pages au même endroit.

### 12.6 🟢
Les zones de regroupement de contenus présentes dans plusieurs pages web (zones d’en-tête, de navigation principale, de contenu principal, de pied de page et de moteur de recherche) peuvent-elles être atteintes ou évitées ? 
> [!TIP] Pas besoin de tester
> Osuny fournit des liens d'accès rapides.

### 12.7 🟢
Dans chaque page web, un lien d’évitement ou d’accès rapide à la zone de contenu principal est-il présent (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Osuny fournit des liens d'accès rapides.

### 12.8 🟢
Dans chaque page web, l’ordre de tabulation est-il cohérent ? 
> [!TIP] Pas besoin de tester
> Osuny fournit un ordre de tabulation correct.

### 12.9 🟢
Dans chaque page web, la navigation ne doit pas contenir de piège au clavier. Cette règle est-elle respectée ? 
> [!TIP] Pas besoin de tester
> Osuny évite les "focus traps".

### 12.10 🟢
Dans chaque page web, les raccourcis clavier n’utilisant qu’une seule touche (lettre minuscule ou majuscule, ponctuation, chiffre ou symbole) sont-ils contrôlables par l’utilisateur ? 
> [!TIP] Pas besoin de tester
> Le thème Osuny n'utilise pas de raccourci clavier.

### 12.11 🟢
Dans chaque page web, les contenus additionnels apparaissant au survol, à la prise de focus ou à l’activation d’un composant d’interface sont-ils si nécessaire atteignables au clavier ? 
> [!TIP] Pas besoin de tester
> Tous les contenus sont accessibles au clavier.

## 13. Consultation

### 13.1 🟢
Pour chaque page web, l’utilisateur a-t-il le contrôle de chaque limite de temps modifiant le contenu (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Le bloc "Témoignages" est le seul bloc avec gestion du temps, et on peut arrêter et contrôler le composant.

### 13.2 🟢
Dans chaque page web, l’ouverture d’une nouvelle fenêtre ne doit pas être déclenchée sans action de l’utilisateur. Cette règle est-elle respectée ? 
> [!TIP] Pas besoin de tester
> Il n'y a aucune popin dans le thème Osuny.

### 13.3 🔴
Dans chaque page web, chaque document bureautique en téléchargement possède-t-il, si nécessaire, une version accessible (hors cas particuliers) ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 13.4 🔴
Pour chaque document bureautique ayant une version accessible, cette version offre-t-elle la même information ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 13.5 🔴
Dans chaque page web, chaque contenu cryptique (art ASCII, émoticône, syntaxe cryptique) a-t-il une alternative ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 13.6 🔴
Dans chaque page web, pour chaque contenu cryptique (art ASCII, émoticône, syntaxe cryptique) ayant une alternative, cette alternative est-elle pertinente ? 
> [!CAUTION] Tester normalement
> Ce critère dépend de la contribution.

### 13.7 🟢
Dans chaque page web, les changements brusques de luminosité ou les effets de flash sont-ils correctement utilisés ? 
> [!TIP] Pas besoin de tester
> Le thème Osuny ne fait pas d'animation.

### 13.8 🟢
Dans chaque page web, chaque contenu en mouvement ou clignotant est-il contrôlable par l’utilisateur ? 
> [!TIP] Pas besoin de tester
> Le thème Osuny ne clignote pas.

### 13.9 🟢
Dans chaque page web, le contenu proposé est-il consultable quelle que soit l’orientation de l’écran (portrait ou paysage) (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Le thème Osuny est consultable dans toutes les orientations.

### 13.10 🟢
Dans chaque page web, les fonctionnalités utilisables ou disponibles au moyen d’un geste complexe peuvent-elles être également disponibles au moyen d’un geste simple (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Le thème Osuny ne nécessite pas de geste complexe.

### 13.11 🟢
Dans chaque page web, les actions déclenchées au moyen d’un dispositif de pointage sur un point unique de l’écran peuvent-elles faire l’objet d’une annulation (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Le thème Osuny ne présente pas ce cas.

### 13.12 🟢
Dans chaque page web, les fonctionnalités qui impliquent un mouvement de l’appareil ou vers l’appareil peuvent-elles être satisfaites de manière alternative (hors cas particuliers) ? 
> [!TIP] Pas besoin de tester
> Le thème Osuny ne présente pas ce cas.
