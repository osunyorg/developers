---
title: Citations
---

## Piste utilisée

En se basant sur le site de Cairn.info, on ne liste que les attributs qui semblent nécessaires pour en dégager le Hash utilisable par CiteProc, et on utilise les attributs présents sur les modèles `Research::Journal::Paper` et `Research::Hal::Publication` pour le créer.

Exemples :

```ruby
{
  "title"=>"Sites de rencontre en ligne : comment se gamifie l’amour 2.0 !",
  "author"=>[
    {"family"=>"Dulaurans", "given"=>"Marlène"},
    {"family"=>"Marczak", "given"=>"Raphaël"}
  ],
  "URL"=>"https://example.org/papiers/2019-12-15-sites-de-rencontre-en-ligne-comment-se-gamifie-lamour-20/",
  "DOI"=>"10.4000/communicationorganisation.8466",
  "container-title"=>"Communication & Organisation",
  "volume"=>56,
  "keywords"=>"",
  "pdf"=>"https://osuny-dev.s3.fr-par.scw.cloud/...",
  "month-numeric"=>"12",
  "issued"=>{
    "date-parts"=>[[2019, 12]]
  },
  "id"=>"59ae40eb-a582-457a-bf15-84b0e49d5686"
}
```

```ruby
{
  "title"=>"Sistemas de evaluación del profesorado en Francia",
  "author"=>[
    {"family"=>"Dulaurans", "given"=>"Marlène"}
  ],
  "URL"=>"https://hal.science/hal-02455890",
  "container-title"=>nil,
  "pdf"=>nil,
  "month-numeric"=>"10",
  "issued"=>{
    "date-parts"=>[[2011, 10]]
  },
  "id"=>"2455890"
}
```

On peut ensuite générer les citations. En comparaison avec Cairn.info :

- APA
  - Cairn.info
    > Dulaurans, M. & Marczak, R. (2019). Sites de rencontre en ligne : comment se gamifie l’amour 2.0 !. Communication & Organisation, 56, 111-122. https://doi.org/10.4000/communicationorganisation.8466
  - Osuny
    > Dulaurans, M., & Marczak, R. (2019). Sites de rencontre en ligne : comment se gamifie l’amour 2.0 ! In Communication & Organisation (Vol. 56). https://doi.org/10.4000/communicationorganisation.8466
- MLA
  - Cairn.info
    > Dulaurans, Marlène, et Raphaël Marczak. « Sites de rencontre en ligne : comment se gamifie l’amour 2.0 ! », Communication & Organisation, vol. 56, no. 2, 2019, pp. 111-122.
  - Osuny
    > Dulaurans, Marlène, et Raphaël Marczak. « Sites de rencontre en ligne : comment se gamifie l’amour 2.0 ! » Communication & Organisation, vol. 56, décembre 2019, https://doi.org/10.4000/communicationorganisation.8466.
- ISO 690
  - Cairn.info
    > DULAURANS Marlène, MARCZAK Raphaël, « Sites de rencontre en ligne : comment se gamifie l’amour 2.0 ! », Communication & Organisation, 2019/2 (n° 56), p. 111-122. DOI : 10.4000/communicationorganisation.8466. URL : https://www.cairn.info/revue-communication-et-organisation-2019-2-page-111.htm
  - Osuny
    > DULAURANS, Marlène et MARCZAK, Raphaël, 2019. Sites de rencontre en ligne : comment se gamifie l’amour 2.0 ! [en ligne]. décembre 2019. Disponible à l'adresse : https://example.org/papiers/2019-12-15-sites-de-rencontre-en-ligne-comment-se-gamifie-lamour-20/

## Ancienne piste

Après exploration, on peut utiliser la librairie [CiteProc](https://github.com/inukshuk/citeproc-ruby) qui permet de générer des citations.

HAL permet de récupérer un fichier BibTeX qui peut servir pour générer un Hash utilisable pour CiteProc.

En analysant plusieurs fichiers BibTeX sur plusieurs publications, on peut en déduire des champs.

Exemple de BibTeX ([source](https://hal.science/hal-03655311v1/bibtex)) :

```
@inproceedings{dulaurans:hal-03655311,
  TITLE = {{Cyberharc{\`e}lement et communaut{\'e}s en ligne : les r{\'e}siliences organisationnelles en jeu !}},
  AUTHOR = {Dulaurans, Marl{\`e}ne and Fedherbe, Jean Christophe},
  URL = {https://hal.science/hal-03655311},
  BOOKTITLE = {{Un monde de crises au prisme des communications organisationnelles}},
  ADDRESS = {Mons, Belgium},
  ORGANIZATION = {{Universit{\'e} Catholique de Louvain = Catholic University of Louvain [UCL]}},
  YEAR = {2022},
  MONTH = May,
  KEYWORDS = {cyberharc{\`e}lement ; recherche action ; flaming ; communaut{\'e}},
  PDF = {https://hal.science/hal-03655311/file/404615.pdf},
  HAL_ID = {hal-03655311},
  HAL_VERSION = {v1},
}
```

Liste des champs
- TITLE : Titre de la publication
  - actuellement `Research::Hal::Publication#title`
- AUTHOR : Auteurs de la publication, au format `NOM1, PRENOM1 and NOM2, PRENOM2`
  - à implémenter à partir de `HalOpenscience::Document#authLastName_s` et `HalOpenscience::Document#authFirstName_s`
- URL : URL HAL de la publication
  - actuellement `Research::Hal::Publication#hal_url`
- BOOKTITLE : Nom de la conférence ou de la revue
  - actuellement `HalOpenscience::Document#bookTitle_s` ou `HalOpenscience::Document#conferenceTitle_s`
- ADDRESS : Lieu de la conférence ou de la revue, au format `VILLE, PAYS`
  - actuellement `HalOpenscience::Document#city_s` et `HalOpenscience::Document#country_s` à convertir du format ISO au nom du pays
- ORGANIZATION : Organisateur de la conférence
  - actuellement `HalOpenscience::Document#conferenceOrganizer_s`
- YEAR : Année de la publication
  - actuellement `Research::Hal::Publication#publication_date.year`
- MONTH : Mois de la publication
  - actuellement `Research::Hal::Publication#publication_date.month.strftime('%b')`
- KEYWORDS : Mots-clés de la publication, séparés de `;`
  - actuellement `HalOpenscience::Document#keyword_s.join(' ; ')`
- PDF : PDF de la publication
  - actuellement `Research::Hal::Publication#file`
- NOTE : Commentaire de la publication
  - actuellement `HalOpenscience::Document#comment_s` ou `{Poster}` quand le document est un poster
- PUBLISHER : Editeur de la revue
  - actuellement `HalOpenscience::Document#publisher_s` ou `HalOpenscience::Document#journalPublisher_s`
- PAGES : Pages de la publication
  - actuellement `HalOpenscience::Document#page_s`
- JOURNAL : Journal de la publication
  - actuellement `HalOpenscience::Document#journalTitle_s`
- HOWPUBLISHED : Contexte d'un poster
  - actuellement `HalOpenscience::Document#conferenceTitle_s`
- NUMBER : Numéro de la revue
  - actuellement : `HalOpenscience::Document#issue_s`
- EDITOR : Éditeur scientifique
  - actuellement : `HalOpenscience::Document#scientificEditor_s`

D'autres sources de BibTeX :
- https://hal.science/hal-03881942v1/bibtex
- https://hal.science/hal-02796816v1/bibtex
- https://shs.hal.science/hal-02148990v1/bibtex
- https://shs.hal.science/hal-03537615v1/bibtex
- https://shs.hal.science/hal-03044620v1/bibtex

### POC

```ruby
require 'bibtex'
require 'citeproc'
require 'csl/styles'

input = BibTeX.open('./hal-02481625v1.bib').to_citeproc

cp_apa = CiteProc::Processor.new style: 'apa', format: 'text'
cp_apa.import input
puts cp_apa.render :bibliography, id: input[0]["id"]
# => Dulaurans, M., & Fedherbe, J.-C. (2020, April). {L'intime, le sensible et le d{'e}lictueux : Se confronter scientifiquement aux terrains du cyberharc{`e}lement}. {Colloque Le Chercheur.e.s Face Au(x) Terrain(s)}. https://hal.science/hal-02481625

cp_iso = CiteProc::Processor.new style: 'iso690-author-date-fr-no-abstract', format: 'text'
cp_iso.import input
puts cp_iso.render :bibliography, id: input[0]["id"]
# => DULAURANS, Marl{`e}ne and FEDHERBE, Jean-Christophe, 2020. {L'intime, le sensible et le d{'e}lictueux : Se confronter scientifiquement aux terrains du cyberharc{`e}lement}. In : {Colloque Le chercheur.e.s face au(x) terrain(s)} [en ligne]. Rouen, France : {Universit{'e} de Rouen Normandie and (REPORTE A UNE DATE ULTERIEURE)}. April 2020. Disponible à l'adresse : https://hal.science/hal-02481625

cp_mla = CiteProc::Processor.new style: 'modern-language-association', format: 'text'
cp_mla.import input
puts cp_mla.render :bibliography, id: input[0]["id"]
# => Dulaurans, Marl{`e}ne, and Jean-Christophe Fedherbe. “{L'Intime, Le Sensible Et Le d{'e}Lictueux : Se Confronter Scientifiquement Aux Terrains Du Cyberharc{`e}Lement}.” {Colloque Le Chercheur.e.s Face Au(x) Terrain(s)}, {Universit{'e} de Rouen Normandie and (REPORTE A UNE DATE ULTERIEURE)}, 2020, https://hal.science/hal-02481625.
```

Cependant :
- Il y a un problème d'échappement des caractères sur les BibTeX renvoyés par HAL
- Il faut pouvoir gérer les papiers de revues scientifiques
- Si les données changent côté Osuny, la citation doit changée