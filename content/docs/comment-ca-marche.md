---
title: Comment ça marche ?
weight: 1
---

## Schéma

TODO dessiner un schéma de l'articulation entre les différentes briques :

L'admin fournit des fichiers qui vont être déposé sur le répertoire git, le répertoire est créé à partir du template qui repose sur le thème.

On peut override les fichiers du thème et les envoyer sur le répertoire git.

Le contenu du répertoire git est déployé automatiquement sur le serveur d'hébergement par une action (github action par exemple). 

{{< cards >}}
  {{< card  link="/docs/website/" 
            image="/images/home/website.jpg"
            title="Développer un site" 
            subtitle="Intégrer un site Web accessible et sobre avec Osuny, pour produire votre propre site Web" >}}
  {{< card  link="/docs/audit/" 
            image="/images/home/audit.jpg"
            title="Auditer un site" 
            subtitle="Réaliser l'audit RGAA d'un site créé avec Osuny, en partant de l'audit socle du thème" >}}
            {{< card  link="/docs/theme/" 
            image="/images/home/theme.jpg"
            title="Contribuer au thème Osuny" 
            subtitle="Partager des améliorations du thème Hugo Osuny avec toute la communauté" >}}
  {{< card  link="/docs/blocks/" 
            image="/images/home/blocks.jpg"
            title="Comprendre les blocs" 
            subtitle="Les blocs sont les briques de base d'Osuny, ils font le pont entre l'admin et le thème" >}}
  {{< card  link="/docs/admin/" 
            image="/images/home/admin.jpg"
            title="Contribuer au CMS Osuny" 
            subtitle="Améliorer l'accessibilité, la sécurité ou développez de nouvelles fonctionnalités dans le système de gestion du contenu (CMS)" >}}
{{< /cards >}}


## Intégration continue

Osuny s'appuie sur une chaîne d'intégration continue.

### Osuny → référentiels Git

La mise à jour d'un site est déclenchée par :
- une mise à jour du contenu envoyée par le back-office Osuny.
- une mise à jour du thème envoyée par le back-office Osuny.
- une mise à jour directe du repository (par exemple lorsqu'on travaille sur le site et qu'on fait évoluer le style).

Dans tous ces cas le repository git (GitHub / GitLab) est mis à jour et déclenche l'étape 2, la mise à jour des sites Web.
En cas de plantage de cette étape, on est prévenus par la levée d'une exception au niveau de l'application Osuny (remontée dans Bugsnag / Slack), ou par un rejet du push manuel.


### Git → sites Web

Il y a des process différents selon le type de repository et/ou le type d'hébergement.

Au niveau du repository on va créer un workflow (GitHub Action ou GitLab job) qui sera exécuté à chaque push. Le workflow est découpé en 2 parties : la compilation du site et l'envoi sur l'hébergement. On est prévenu en cas de plantage de l'une ou l'autre de ces parties.
