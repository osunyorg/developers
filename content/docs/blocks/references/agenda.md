---
title: Agenda
weight: 5
---

![image](https://raw.githubusercontent.com/osunyorg/admin/refs/heads/main/app/assets/images/communication/blocks/templates/agenda.jpg)

```yaml {filename="Données Hugo"}
  - kind: block
    template: agenda
    title: >-
      À venir
    slug: >-
      a-venir
    ranks:
      self: 2
      children: 3
    data:
      mode: all
      options:
        categories: false
        dates: true
        image: true
        subtitle: true
        summary: true
        status: false

      layout: grid
      description: >-
        
      no_event_message: >-
        
      title_link: "/agenda/"
      events:
        - permalink: "/agenda/2024-11-27-communs-numerique-et-interet-general/"
          path: "/events/2024-12-27-communs-numerique-et-interet-general"
          slug: "communs-numerique-et-interet-general"
          file: "content/fr/events/2024-12-27-communs-numerique-et-interet-general.html"
```