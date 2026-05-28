---
title: Forges
weight: 1
description: >
  Publier le site à partir d'une forge (GitHub, GitLab, ...)
---

Le code du site est hébergé sur une forge, un outil pour gérer des projets numériques collaboratifs. GitHub (propriété de Microsoft) est la forge la plus utilisée dans le monde, mais des alternatives existent, tels que GitLab ou encore Forgejo, des modèles open-source permettant de les auto-héberger pour les personnes plus techniques.

## Préférer le SSH

Pour chacune des forges, nous présenterons des scripts supportant différents protocoles d'envoi de fichiers. Les protocoles les plus courants sont SSH et FTP.
Il faut systématiquement privilégier le SSH, qui présente de meilleures performances.
Lors d'un test nous avons constaté que le même site est mis à jour en 20 secondes via SSH et en 3 minutes via FTP.
