---
title: Intégration HTML
weight: 6
---

![image](https://raw.githubusercontent.com/osunyorg/admin/refs/heads/main/app/assets/images/communication/blocks/templates/embed.jpg)

```yaml {filename="Données Hugo"}
  - kind: block
    template: embed
    title: >-
      
    slug: >-
      
    ranks:
      self: 2
    data:
      code: >-
        <iframe src="https://example.edu" loading="lazy"></iframe>
      transcription_title: >-
        
      transcription: >-
        
```

L'attribut `loading="lazy"` est ajouté automatiquement.