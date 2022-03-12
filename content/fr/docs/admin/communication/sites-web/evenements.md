---
title: Evénements
---

Est-ce vraiment du domaine du site ? Ou est-ce plus haut, avec une publication sur le site ?

## Modèles

### communication/website/Event

- university:references
- communication_website:references
- communication_website_event_kind:references
- name:string
- description:text
- text:html
- published:boolean
- published_at:datetime

### communication/website/event/Kind

- university:references
- communication_website:references
- name:string
- position:integer
