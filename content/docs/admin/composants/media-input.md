---
title: Choix de média
---

![](media-input.png)

Le choix de media est un composant qui gère les 3 sources possibles de médias : 
- l'envoi direct
- le choix Unsplash / Pexels
- le choix dans la photothèque

Les 3 choix aboutissent au même résultat : un identifiant de média.

```mermaid
graph TD;
  ModeChoix-->Upload-->Crop-->FindOrCreateMedia-->IdMedia
  ModeChoix-->UnsplashPexels-->FindOrCreateMedia-->IdMedia
  ModeChoix-->Phototheque-->IdMedia

  ModeChoix{"Quel mode d'envoi ?"}
  Upload["Upload de fichier"]
  UnsplashPexels["Unsplash ou Pexels"]
  Phototheque["Photothèque"]
  Crop["Recadrage"]
  FindOrCreateMedia["Intégration à la médiathèque"]
  IdMedia["Identifiant du média"]
```