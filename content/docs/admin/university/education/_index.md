---
title: Enseignement
weight: 10
---

## Modèles

### Formation

> Exemple: Bachelor Universitaire de Technologie Métiers du Multimédia et de l'Internet

https://schema.org/EducationalOccupationalProgram


```
# Table name: education_programs
#
#  id                 :uuid             not null, primary key
#  accessibility      :text
#  capacity           :integer
#  contacts           :text
#  content            :text
#  continuing         :boolean
#  description        :text
#  duration           :text
#  ects               :integer
#  evaluation         :text
#  featured_image_alt :string
#  level              :integer
#  name               :string
#  objectives         :text
#  opportunities      :text
#  other              :text
#  path               :string
#  pedagogy           :text
#  position           :integer          default(0)
#  prerequisites      :text
#  pricing            :text
#  published          :boolean          default(FALSE)
#  registration       :text
#  results            :text
#  slug               :string
#  created_at         :datetime         not null
#  updated_at         :datetime         not null
#  parent_id          :uuid             indexed
#  university_id      :uuid             not null, indexed
```

https://dossierdoc.typepad.com/descripteurs/2016/05/secteur-de-l-%C3%A9ducation-et-de-la-formation-schemas-de-metadonnees-et-vocab.html

### Ecole

> IUT Bordeaux Montaigne

```
# Table name: education_schools
#
#  id            :uuid             not null, primary key
#  address       :string
#  city          :string
#  country       :string
#  latitude      :float
#  longitude     :float
#  name          :string
#  phone         :string
#  zipcode       :string
#  created_at    :datetime         not null
#  updated_at    :datetime         not null
#  university_id :uuid             not null, indexed
```

### Cours

> Exemple: 30 heures d'histoire de l'art en année 1

```
education/Course
- program:references
- academic_year:references
- name:string
- syllabus:text
- hours:integer
```
