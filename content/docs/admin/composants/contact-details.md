---
title: Informations de contact
---

## Service

TODO expliquer le fonctionnement de `ContactDetails`.

## Données statiques

### Principe

Dans chaque clé, nous fournissons un label et une value.
Le label est la version nettoyée, minimale, à afficher pour les humains.
La value est le code à utiliser en lien.
Nous procédons, pour chaque format, à une analyse et à un nettoyage afin d'éviter les données incohérentes.

```yaml {filename="Donnée brute"}
facebook:
  label: >-
    noesya.coop
  value: >-
    https://www.facebook.com/noesya.coop
```

```yaml {filename="Lien généré"}
<a href="https://www.facebook.com/noesya.coop"
   title="Consulter la page noesya sur Facebook"
   target="_blank"
   rel="noreferrer">
   noesya.coop
</a>
```

### Schema

```yaml
contact_details:
  postal_address:
    data: # Données brutes
      address: >-
      address_additional: >-
      zipcode: >-
      city: >-
      country: >-
        name: >-
        alpha2: >-
        alpha3: >-
    text: >-
    html: >-
  phone_numbers:
  emails:
  social_networks:
        email:
        facebook:
        github:
        instagram:
        linkedin:
        mastodon:
        peertube:
        vimeo:
        x:
        youtube:

```

### Exemple

```yaml
contact_details:
  postal_address:
    data:
      address: >-
        15 rue des Bouviers
      address_additional: >-
        Sonner en bas
      zipcode: >-
        33800
      city: >-
        Bordeaux
      country: >-
        name: >-
          France
        alpha2: >-
          FR
        alpha3: >-
          FRA
    text: >-
      15 rue des Bouviers 33800 Bordeaux FRANCE
    html: >-
      <address itemprop="address" itemscope itemtype="https://schema.org/PostalAddress"> <span itemprop="streetAddress">15 rue des Bouviers</span> <span itemprop="postalCode">33800</span> <span itemprop="addressLocality">Bordeaux</span> <span itemprop="addressCountry">FRANCE</span></address>
  phone_numbers:
    - label: >-
        06 87 60 21 93
      value: >-
        tel:0687602193
    - label: >-
        +33 6 87 60 21 93
      value: >-
        tel:+33687602193
  emails:
    - label: >-
        administratif@noesya.coop
      value: >-
        mailto:administratif@noesya.coop
    - label: >-
        arnaud.levy@noesya.coop
      value: >-
        mailto:arnaud.levy@noesya.coop
  social_networks:
    facebook:
      label: >-
        noesya.coop
      value: >-
        https://www.facebook.com/noesya.coop
    github:
      label: >-
        noesya
      value: >-
        https://github.com/noesya
    instagram:
      label: >-
        noesya_coop
      value: >-
        https://instagram.com/noesya_coop
    linkedin:
      label: >-
        arnaudlevy
      value: >-
        https://www.linkedin.com/in/arnaudlevy/
    mastodon:
      label: >-
        mastodon.social/@arnaudlevy
      value: >-
        https://mastodon.social/@arnaudlevy
    peertube:
      label: >-
        peertube.designersethiques.org
      value: >-
        https://peertube.designersethiques.org
    tiktok:
      label: >-
        tiktok
      value: >-
        https://www.tiktok.com/@tiktok
    vimeo:
      label: >-
        noesya
      value: >-
        https://vimeo.com/noesya
    x:
      label: >-
        arnaudlevy
      value: >-
        https://x.com/arnaudlevy
    youtube:
      label: >-
        MMIBordeaux
      value: >-
        https://www.youtube.com/@MMIBordeaux
```

## Cas d'usages

### Personnes

```yaml
...
contact_details:
  postal_address:
    data:
      address_name: >-
        
      address: >-
        5 rue Frédéric Joliot Curie
      address_additional: >-
        
      zipcode: >-
        33150
      city: >-
        Cenon
      country: >-
        name: >-
          France
        alpha2: >-
          FR
        alpha3: >-
          FRA
    text: >-
      5 rue Frédéric Joliot Curie 33150 Cenon FRANCE
    html: >-
      <address itemprop="address" itemscope itemtype="https://schema.org/PostalAddress"> <span itemprop="streetAddress">5 rue Frédéric Joliot Curie</span> <span itemprop="postalCode">33150</span> <span itemprop="addressLocality">Cenon</span> <span itemprop="addressCountry">FRANCE</span></address>

  phone_numbers:
    phone_mobile:
      label: >-
        0687602193
      value: >-
        tel:0687602193

    phone_personal:
      label: >-
        0687602193
      value: >-
        tel:0687602193


  emails:
    email:
      label: >-
        arnaud.levy@noesya.coop
      value: >-
        mailto:arnaud.levy@noesya.coop


  website:
    label: >-
      www.noesya.coop
    value: >-
      https://www.noesya.coop


  social_networks:
    linkedin:
      label: >-
        arnaudlevy
      value: >-
        https://www.linkedin.com/in/arnaudlevy/

    mastodon:
      label: >-
        mastodon.social/@arnaudlevy
      value: >-
        https://mastodon.social/@arnaudlevy
...
```

### Organisations
```yaml
...
contact_details:
  postal_address:
    data:
      address_name: >-
        noesya
      address: >-
        15 rue des Bouviers
      address_additional: >-
        Sonner en bas
      zipcode: >-
        33000
      city: >-
        Bordeaux
      country: >-
        name: >-
          France
        alpha2: >-
          FR
        alpha3: >-
          FRA
    geolocation:
      latitude: 44.8328992
      longitude: -0.5654338
    text: >-
      noesya 15 rue des Bouviers Sonner en bas 33000 Bordeaux FRANCE
    html: >-
      <address itemprop="address" itemscope itemtype="https://schema.org/PostalAddress"> <span itemprop="name">noesya</span> <span itemprop="streetAddress">15 rue des Bouviers</span> <span itemprop="description">Sonner en bas</span> <span itemprop="postalCode">33000</span> <span itemprop="addressLocality">Bordeaux</span> <span itemprop="addressCountry">FRANCE</span></address>

  phone_numbers:
    phone:
      label: >-
        0687602193
      value: >-
        tel:0687602193

  emails:
    email:
      label: >-
        arnaud.levy@noesya.coop
      value: >-
        mailto:arnaud.levy@noesya.coop

  website:
    label: >-
      www.noesya.coop
    value: >-
      https://www.noesya.coop

  social_networks:
    linkedin:
      label: >-
        noesyacoop
      value: >-
        https://www.linkedin.com/company/noesyacoop

    mastodon:
      label: >-
        mastodon.social/@noesya
      value: >-
        https://mastodon.social/@noesya
...
````

### Écoles

```yaml
...
contact_details:
  postal_address:
    data:
      address_name: >-
        
      address: >-
        rue de Marne
      address_additional: >-
        
      zipcode: >-
        77400
      city: >-
        Meaux
      country: >-
        name: >-
          France
        alpha2: >-
          FR
        alpha3: >-
          FRA
    geolocation:
      latitude: 
      longitude: 
    text: >-
      rue de Marne 77400 Meaux FRANCE
    html: >-
      <address itemprop="address" itemscope itemtype="https://schema.org/PostalAddress"> <span itemprop="streetAddress">rue de Marne</span> <span itemprop="postalCode">77400</span> <span itemprop="addressLocality">Meaux</span> <span itemprop="addressCountry">FRANCE</span></address>

  phone_numbers:
    phone:
      label: >-
        0687602193
      value: >-
        tel:0687602193

  emails:

  website:
    label: >-
      www.noesya.coop
    value: >-
      https://www.noesya.coop

  social_networks:
...
```

### Laboratoires

```yaml
---
...
contact_details:
  postal_address:
    data:
      address_name: >-
        
      address: >-
        1 rue du Lab
      address_additional: >-
        
      zipcode: >-
        77400
      city: >-
        Meaux
      country: >-
        name: >-
          France
        alpha2: >-
          FR
        alpha3: >-
          FRA
    text: >-
      1 rue du Lab 77400 Meaux FRANCE
    html: >-
      <address itemprop="address" itemscope itemtype="https://schema.org/PostalAddress"> <span itemprop="streetAddress">1 rue du Lab</span> <span itemprop="postalCode">77400</span> <span itemprop="addressLocality">Meaux</span> <span itemprop="addressCountry">FRANCE</span></address>

  phone_numbers:

  emails:


  social_networks:
...
```

### Sites (campus)

```yaml
...
contact_details:
  postal_address:
    data:
      address_name: >-
        Adresse principale
      address: >-
        Campus Périgord – Rond Point Suzanne Noël
      address_additional: >-
        Complément
      zipcode: >-
        33150
      city: >-
        Cenon
      country: >-
        name: >-
          France
        alpha2: >-
          FR
        alpha3: >-
          FRA
    geolocation:
      latitude: 
      longitude: 
    text: >-
      Adresse principale Campus Périgord – Rond Point Suzanne Noël Complément 33150 Cenon FRANCE
    html: >-
      <address itemprop="address" itemscope itemtype="https://schema.org/PostalAddress"> <span itemprop="name">Adresse principale</span> <span itemprop="streetAddress">Campus Périgord – Rond Point Suzanne Noël</span> <span itemprop="description">Complément</span> <span itemprop="postalCode">33150</span> <span itemprop="addressLocality">Cenon</span> <span itemprop="addressCountry">FRANCE</span></address>

  phone_numbers:
    phone:
      label: >-
        0687602193
      value: >-
        tel:0687602193

  emails:

  social_networks:
...
```

### Bloc contact

```yaml
...
contents:
  - kind: block
    template: contact
    ...
    data:
      ...
      contact_details:
        postal_address:
          data:
            address_name: >-
              
            address: >-
              5 rue Frédéric Joliot Curie
            address_additional: >-
              
            zipcode: >-
              33150
            city: >-
              Cenon
            country: >-
              name: >-
                France
              alpha2: >-
                FR
              alpha3: >-
                FRA
          text: >-
            5 rue Frédéric Joliot Curie 33150 Cenon FRANCE
          html: >-
            <address itemprop="address" itemscope itemtype="https://schema.org/PostalAddress"> <span itemprop="streetAddress">5 rue Frédéric Joliot Curie</span> <span itemprop="postalCode">33150</span> <span itemprop="addressLocality">Cenon</span> <span itemprop="addressCountry">FRANCE</span></address>

        phone_numbers:
          list:
            - label: >-
                06 87 60 21 93
              value: >-
                tel:0687602193
            - label: >-
                +33 6 87 60 21 93
              value: >-
                tel:+33687602193

        emails:
          list:
            - label: >-
                administratif@noesya.coop
              value: >-
                mailto:administratif@noesya.coop
            - label: >-
                arnaud.levy@noesya.coop
              value: >-
                mailto:arnaud.levy@noesya.coop


        social_networks:
          facebook:
            label: >-
              noesya.coop
            value: >-
              https://www.facebook.com/noesya.coop

          github:
            label: >-
              noesya
            value: >-
              https://github.com/noesya

          instagram:
            label: >-
              noesya_coop
            value: >-
              https://instagram.com/noesya_coop

          linkedin:
            label: >-
              arnaudlevy
            value: >-
              https://www.linkedin.com/in/arnaudlevy/

          mastodon:
            label: >-
              mastodon.social/@arnaudlevy
            value: >-
              https://mastodon.social/@arnaudlevy

          peertube:
            label: >-
              peertube.designersethiques.org
            value: >-
              https://peertube.designersethiques.org

          tiktok:
            label: >-
              tiktok
            value: >-
              https://www.tiktok.com/@tiktok

          vimeo:
            label: >-
              noesya
            value: >-
              https://vimeo.com/noesya

          x:
            label: >-
              arnaudlevy
            value: >-
              https://x.com/arnaudlevy

          youtube:
            label: >-
              MMIBordeaux
            value: >-
              https://www.youtube.com/@MMIBordeaux
      ...
```

### Site Web

Cela représente les infos de contact d'un site, langue par langue.

```yaml {filename="config/_default/languages.yaml"}
fr:
  ...
  params:
    contact_details:
      social_networks:
        email:
          label: >-
            arnaud.levy@noesya.coop
          value: >-
            mailto:arnaud.levy@noesya.coop
        facebook:
          label: >-
            VillaKujoyama
          value: >-
            https://www.facebook.com/VillaKujoyama
        github:
          label: >-
            villakujoyama
          value: >-
            https://github.com/villakujoyama
        instagram:
          label: >-
            villa_kujoyama
          value: >-
            https://instagram.com/villa_kujoyama
        linkedin:
          label: >-
            villakujoyama
          value: >-
            https://www.linkedin.com/company/villakujoyama/
        mastodon:
          label: >-
            mastodon.social/@arnaudlevy
          value: >-
            https://mastodon.social/@arnaudlevy
        peertube:
          label: >-
            peertube.designersethiques.org
          value: >-
            https://peertube.designersethiques.org
        vimeo:
          label: >-
            villakujoyama
          value: >-
            https://vimeo.com/villakujoyama
        x:
          label: >-
            villakujoyama
          value: >-
            https://x.com/villakujoyama
        youtube:
          label: >-
            villakujoyama
          value: >-
            https://www.youtube.com/@villakujoyama
  ...
```


