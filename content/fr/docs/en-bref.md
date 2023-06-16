---
title: Osuny en bref
weight: 1
description: >
  Comprendre l'architecture d'Osuny
---
Osuny est composé de 2 briques :
- un outil d'administration (ou CMS), qui est une application [Ruby on Rails](https://rubyonrails.org)
- des sites, qui s'appuient sur [Hugo](https://gohugo.io/).

Les extranets sont des cas particuliers, qui s'appuient sur l'application Ruby on Rails.

Chaque site est construit :
- en partant du [Template Osuny](https://github.com/noesya/osuny-hugo-template-aaa)
- qui utilise le [Thème Osuny](https://github.com/noesya/osuny-hugo-theme-aaa)
- configuré et personnalisé à volonté

**Glossaire**

Admin Osuny
: L'application permettant de gérer les contenus. On utilise aussi les synonyme *CMS Osuny* ou *back-office Osuny*.

Template Osuny 
: Le template qui sert de base pour créer des sites avec Osuny.

Thème Osuny
: Le thème Hugo dédié à Osuny, qui permet d'afficher de façon responsive, sobre et accessible les contenus produits avec le CMS Osuny.

Site
: Un site utilisant Osuny, à la fois le template, le thème et l'admin.

