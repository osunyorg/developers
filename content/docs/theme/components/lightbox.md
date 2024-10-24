---
title: Visionneuse
---

## Fonctionnalités
### Contextes
La visionneuse est une option activable dans le fichier `config.yaml` dans deux contextes: 
Le premier contexte est celui d'une image seule, pour lequel si l'option `disabled` est à `false` permet, au click sur toute image du site, de l'ouvrir dans une visionneuse.

``` yaml
params:
  image_sizes:
    design_system:
      lightbox:
        disabled: false #true to disable
```

Le second contexte est celui du bloc galerie pour lequel l'option `disabled: false`, permet, au click sur une image de la galerie, d'ouvrir l'image dans une visionneuse et de naviguer entre les différentes images composant la galerie.

``` yaml
params:
  image_sizes:
    design_system:
      gallerylightbox:
        disabled: false #true to disable
``` 
### Affichage

![](images/lightbox-image-seule.png "Visionneuse ouverte contenant une seule image en affichage desktop")
![](images/lightbox-image-seule-mobile.png "Visionneuse ouverte contenant une seule image en affichage mobile")

![](images/lightbox-gallery.png "Visionneuse ouverte contenant plusieurs images (galerie) en affichage desktop")
![](images/lightbox-gallery-mobile.png "Visionneuse ouverte contenant plusieurs images (galerie) en affichage mobile")

### Comportement
L'ouverture de la visionneuse se fait au click sur une image, suite auquel l'image apparait dans un conteneur prenant toute la taille de l'écran.
Une fois la visionnneuse ouverte, la fermeture peut se faire de plusieurs manières: 
- Au click sur le bouton en forme de croix affiché en bas à droite de la visionneuse
- Au click sur l'espace autour de l'image dans la visionneuse
- En appuyant sur la touche `escape` du clavier

Dans le cas d'une galerie, la navigation entre les images se fait de plusieurs manières: 
- Au click sur un des boutons en forme de flèche (gauche/droite) affiché en bas à droite de la visionneuse
- En appuyant sur la touche `arrowLeft` ou `arrowRight` du clavier

Dans Osuny, les images peuvent être associées d'un crédit et d'une description.
Si l'une, l'autre, ou les deux de ces informations sont disponibles, le ou les boutons "©" pour le crédit et "i" pour la description, apparaissent en bas à droite de l'écran.

Au click sur un de ces boutons, le contenu de l'information est ouvert dans une fenêtre de type pop-in.
Une fois ouverte, cette pop-in peut être fermée en cliquant sur le bouton "fermer", ou bien en cliquant sur le bouton "©" ou "i" activé.

![](images/lightbox-gallery-popup.png "Visionneuse avec la fenêtre de description ouverte.")

## Implémentation
{{< callout type="info" >}}
  Les 3 premiers objets sont par ordre logique.
{{< /callout >}}

### Manager
Manager est chargé de gérer les différentes visionneuses d'une page. 
Si il existe au moins une image avec l'option visionneuse présente dans la page, le manager instancie un objet de la classe `Container`.
Ensuite il gère les évènements sur la page.

Lorsqu'une image peut s'ouvrir en visionneuse, elle est entourée d'un lien qui au click déclenche l'évènement d'ouverture de la visionneuse par le `Manager`.
Ce lien est dissimulé pour les technologies d'assistance avec un `aria-hidden= "true"` et non focusable par son `tabindex=-1`.
``` HTML
<figure role="figure" class="image-landscape lightbox-figure">
  <a class="lightbox-launcher" role="button" aria-hidden="true" tabindex="-1" data-lightbox="{}" href="[imgurl]" value="0">
    <span class="sr-only">Agrandir l'image</span>
    <picture> ... </picture>
  </a>
</figure>
```
Au click sur une image, le `manager` déclenche l'ouverture du container dont il va charger le contenu en fonction de l'image activée.
Ce contenu est encapsulé dans un objet de classe `Lightbox`.

``` javaScript {filename="manager.js"}
    _onLauncherClick (event) { // au click, on récupère l'index de l'image dans la page
        var index = event.currentTarget.getAttribute('value');
        this._setLightboxContent(index); // le contenu est mis à jour
        this._open(); // la visionneuse s'ouvre
    },
    _setLightboxContent (index) {
        this.currentLightbox = this.lightboxes[index];
        this.container.show(this.currentLightbox);
    },
    _open () {
        if (!this.container.opened) {
            this.container.open(); 
        }
    },
```

Le manager intercepte également les évènements `previous`, `next`, `close`, qu'il restransmet au `Container`.

``` javaScript {filename="manager.js"}
    next () {
        if (this.currentLightbox.next) { // si l'instance de Lightbox a une image suivante
            this._setLightboxContent(this.currentLightbox.next); // le contenu est mis à jour avec le contenu de l'image suivante
        }
    },
    previous () {
        if (this.currentLightbox.previous) {
            this._setLightboxContent(this.currentLightbox.previous);
        }
    },
```

À la fermeture de la visionneuse, le focus est remis sur le lien de l'image.
``` javaScript {filename="manager.js"}
    close () {
        this.container.close();
        this.currentLightbox.launcher.focus();
    },
```



### Container
`Container` est l'élément qui s'affiche en modale ou non.
Son code HTML est composé d'une `<figure>` dont le contenu est l'image ou vide si il est fermé. Et d'un `<div>` contenant les différents boutons de contrôle, ainsi que la fenêtre pop-in, mis à jours en fonction du contenu.

Lorsqu'il est fermé, le container est caché en CSS.
``` HTML
<div class="lightbox-container">
  <figure role="figure" class="lightbox-content" tabindex="0"></figure>
  <div class="lightbox-controls">
    ...
  </div>
</div>
```

Lors qu'il est ouvert, le container est affiché en CSS et la figure est chargée de l'`<img>` correspondant.
```HTML 
<div class="lightbox-container" style="display: block;">
  <figure role="figure" class="lightbox-content" tabindex="0" aria-label="Magazine">
    <img src="..." alt="Magazine">
  </figure>
  <div class="lightbox-controls">
      ...
  </div>
</div>
```

Comme vu précédemment, le manager lui injecte le contenu de l'image correspondante.
Il contient les fonctions `open()` et `close()` qui vont changer son état d'affichage, `show()` qui va charger les informations de l'image (`Lightbox`) à afficher.
Il instancie aussi l'interface de contrôles ainsi que la fenêtre d'affichage des informations et crédits.

À l'ouverture de la visionneuse, tous les éléments interactifs sont rendus non focusables conformément au critère RGAA 10.8.
Les évènements souris et clavier sont écoutés pour déclencher la fermeture.

> [**Critère d'accessibilité 10.8**](https://accessibilite.numerique.gouv.fr/methode/criteres-et-tests/#10.8): Pour chaque page web, les contenus cachés ont-ils vocation à être ignorés par les technologies d’assistance ? 
``` javaScript {filename="container.js"}
    open () {
        this._setPageElementsEnabled(false); 
        this.bodyElement.style.overflow = 'hidden';
        this.element.style.display = 'block';
        this.element.addEventListener('click', this.eventsCallback.click); 
        document.addEventListener('keydown', this.eventsCallback.keyDown);
        this.opened = true;
    },
    _setPageElementsEnabled (enabled) {
        this.pageFocusableElements.forEach(function (elem) {
            this._setPageElementEnabled(elem, enabled);
        }.bind(this));
    },
    _setPageElementEnabled (elem, enabled) {
      var element = document.querySelector(elem);
      element.querySelectorAll('a, button, iframe').forEach(function (e) {
          e.setAttribute('tabindex', enabled ? '0' : '-1');
      });
      element.setAttribute('tabindex', enabled ? '0' : '-1');
      element.setAttribute('aria-hidden', String(!enabled));
    },
```

À la fermeture de la visionneuse, le contenu est supprimé, les prises de focus des éléments interactifs sont réactivés et les listeners sont supprimés. 

``` javaScript {filename="container.js"}
    close () {
        this._removeImageContent();
        this._setPageElementsEnabled(true);
        this.bodyElement.style.overflow = 'visible';
        this.element.style.display = 'none';
        this._closePopup();
        this.element.removeEventListener('click', this.eventsCallback.click);
        document.removeEventListener('keydown', this.eventsCallback.keyDown);
        this.opened = false;
    },
```

La fonction `show(lightbox)` invoquée par le manager met le contenu à jour.
D'abord, il supprime l'éventuel contenu de la pop-in, ensuite il supprime le contenu de l'image, puis il met à jour le contenu de la nouvelle image ainsi que le rack de boutons de contrôle et la fenêtre pop-in.
L'attribut `alt` de l'image ainsi que l'attribut aria-label sont mis à jour avec la description ou le credit de l'image si ils existent.
Puis le focus est forcé sur la nouvelle image venant d'apparaître à l'écran.

> [**Critère d'accessibilité 1.1**](https://accessibilite.numerique.gouv.fr/methode/criteres-et-tests/#1.1): Chaque image porteuse d’information a-t-elle une alternative textuelle ? 

``` javaScript {filename="container.js"}
    show (lightbox) {
        this.popupDetails.close();
        this._removeImageContent();
        this._setImageContent(lightbox);
        this.controlRack.load(lightbox);
        this.popupDetails.load(lightbox);
    },
    _setImageContent (lightbox) {
      var image = document.createElement('img'),
          imageDescription = lightbox.descriptionPlain || lightbox.creditPlain || '';
      this._closePopup();
      image.setAttribute('src', lightbox.url);
      image.setAttribute('alt', imageDescription); 
      this.content.setAttribute('aria-label', imageDescription);
      this.content.append(image);
      this.content.focus(); 
    },
```

### Lightbox
L'instance de lightbox représente une image de la page et ses informations associées (dont éventuellement le crédit et la description).
Elle détermine également si l'image a une image précédente / suivante ou non et son index de position parmi cette liste d'image

{{< callout type="info" >}}
  Les objets suivants sont par ordre alphabétique.
{{< /callout >}}

### Classes
Liste des classes HTML utilisées dans le DOM.

### Controls
C'est l'interface de contrôle de la visionneuse.
Elle dispose, des boutons suivants :
- Information : si l'image dispose d'une description
- Crédit : si l'image dispose d'un crédit
- flèche de gauche : s'il y a une image précédente
- Flèche de droite : s'il y a une image suivante
- Fermer : toujours présent

Lors d'un clic sur un bouton, il déclenche un événement correspondant


``` HTML
    <button class="info">
      <span class="sr-only">Afficher les informations de l'image</span>
    </button>
    <button class="credit">
      <span class="sr-only">Afficher les crédits de l'image</span>
    </button>
    <button class="prev">
      <span class="sr-only">Aller à l'image précédente</span>
    </button>
    <button class="next">
      <span class="sr-only">Aller à l'image suivante</span>
    </button>
    <button class="close">
      <span class="sr-only">Fermer la lightbox</span>
    </button>
```
### Events
Liste des événements Javascript émis.

### PopupDetails
Fenêtre d'affichage des informations ou dex crédits. 
Le popup est mise à jour à chaque changement d'image `load()`
Elle peut montrer les crédits ou description `show(content)`, et se fermer `close()`.

``` HTML
    <div class="details-window" tabindex="0" style="display: none;">
      <div>
        <p class="details-window-title-information">Information</p>
        <p class="details-window-title-credit">Crédit</p>
        <a class="details-window-close" href="">Fermer</a>
      </div>
      <p class="details-window-content"></p>
    </div>
```