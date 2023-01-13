---
title: HAL
---

## Architecture

Les chercheurs et chercheuses sont supposées partager leurs publications sur HAL.
En indexant les documents dans Osuny automatiquement, on peut mettre à jour les bibliographies automatiquement.

Une première approche consiste à lister des "Documents", qui appartiennent à la personne. C'est simple, mais ça crée des doublons (un article co-signé crée 2 documents).

```
research/Document
- university:references
- university_person:references
- docid:string
- data:jsonb
- title:string
- url:string
- ref:string
```

Une seconde approche, plus robuste, consisterait à utiliser au niveau d'Osuny (pour toutes les universités à la fois), des documents qui sont reliés aux personnes, aux laboratoires, aux écoles, aux revues... On peut comme ça centraliser les mises à jour.

```
research/Document
- docid:string
- data:jsonb
- title:string
- url:string
- ref:string

research_documents_university_people
- research_document:references
- university_person:references

research_documents_research_laboratories
- research_document:references
- research_laboratory:references

education_schools_research_documents
- education_school:references
- research_document:references
```

## API

https://api.archives-ouvertes.fr/docs
### Auteurs
https://api.archives-ouvertes.fr/ref/author?q=Marl%C3%A8ne%20Dulaurans&fl=*

```json
{
  "response": {
    "numFound": 2,
    "start": 0,
    "numFoundExact": true,
    "docs": [
      {
        "docid": "1375936-0",
        "form_i": 1375936,
        "firstName_s": "Marlène",
        "lastName_s": "Dulaurans",
        "fullName_s": "Marlène Dulaurans",
        "fullName_sci": "Marlène Dulaurans",
        "fullNameDocid_fs": "Marlène Dulaurans_FacetSep_1375936-0",
        "label_html": "<span>Marlène Dulaurans</span>",
        "valid_s": "INCOMING",
        "_version_": 1750727780673257500,
        "dateLastIndexed_tdate": "2022-11-28T08:28:31.085Z"
      },
      {
        "person_i": 1063453,
        "form_i": 1375936,
        "docid": "1375936-1063453",
        "firstName_s": "Marlène",
        "lastName_s": "Dulaurans",
        "middleName_s": "",
        "fullName_s": "Marlène Dulaurans",
        "fullName_sci": "Marlène Dulaurans",
        "label_s": "Marlène Dulaurans",
        "fullNameDocid_fs": "Marlène Dulaurans_FacetSep_1375936-1063453",
        "label_html": "<span class='preferred'>Marlène Dulaurans  <span class=\"address\">@hotmail.com</span>     </span>",
        "valid_s": "PREFERRED",
        "emailId_s": [
          "b49e3eacda8115c85ea9ab8384e3d770"
        ],
        "emailDomain_s": [
          "hotmail.com"
        ],
        "hasCV_bool": false,
        "_version_": 1750727780672209000,
        "dateLastIndexed_tdate": "2022-11-28T08:28:31.085Z"
      }
    ]
  }
}
```

### Publications

https://api.archives-ouvertes.fr/search/?q=authIdPerson_i:1063453

```json
{
  "response":
    {
      "numFound":51,
      "start":0,
      "maxScore":1.0,
      "numFoundExact":true,
      "docs":[
        {
          "docid":"2498602",
          "label_s":"Marlène Dulaurans, Jean-Christophe Fedherbe. Cyber-harassment and social networks : the CyberNeTic research project. SUNBELT 2020, Session Recent advances in Social Network Analysis, Jun 2020, Paris, France. &#x27E8;hal-02498602&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02498602"},
        {
          "docid":"2455876",
          "label_s":"Marlène Dulaurans. Les métamorphoses du lien social par l'innovation digitale. Journées d'étude internationales du Laboratoire du lien social: Critique de la précarité, politiques de la solidarité, Université Bordeaux Montaigne, Dec 2013, Bordeaux, France. &#x27E8;hal-02455876&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455876"},
        {
          "docid":"2455885",
          "label_s":"Marlène Dulaurans. Cyberharcèlement et éducation aux médias. Aquinum, Oct 2018, Bordeaux, France. &#x27E8;hal-02455885&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455885"},
        {
          "docid":"2455807",
          "label_s":"Marlène Dulaurans. Négociation. Valérie Carayol; Gino Gramaccia. Abécédaire. Vingt ans de recherches et de publications en communication des organisations, 2013. &#x27E8;hal-02455807&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455807"},
        {
          "docid":"2455857",
          "label_s":"Marlène Dulaurans, Raphael Marczak. Confiden-CIEL, Retours expérientiels essentiels d'un ludiciel pour les compétences informationnelles. Journées d'études internationales Repenser l’acquisition des compétences informationnelles en direction des « digital na(t)ives » : définitions, innovation pédagogique, écologie de l’attention et accès à l’environnement numérique, Université Bordeaux Montaigne, Nov 2019, Bordeaux, France. &#x27E8;hal-02455857&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455857"},
        {
          "docid":"2455806",
          "label_s":"Marlène Dulaurans. Médiation. Valérie Carayol; Gino Gramaccia. Abécédaire. Vingt ans de recherches et de publications en communication des organisations, 2013. &#x27E8;hal-02455806&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455806"},
        {
          "docid":"2455811",
          "label_s":"Marlène Dulaurans, Raphael Marczak. Quand le jeu vidéo devient affaire de clan : un Clash Royale entre incitation et inhibition. Revue française des sciences de l'information et de la communication, 2018, Religions et médias, 13, &#x27E8;10.4000/rfsic.3610&#x27E9;. &#x27E8;hal-02455811&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455811"},
        {
          "docid":"2455845",
          "label_s":"Marlène Dulaurans, Jean-Christophe Fedherbe. \"J'aurais préféré que personne ne le sache !\", Comment se reconstruire après le cyberharcèlement ?. 88ème Congrès de l'ACFAS, Université de Sherbrooke, May 2020, Sherbrooke, Canada. &#x27E8;hal-02455845&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455845"},
        {
          "docid":"2455865",
          "label_s":"Marlène Dulaurans. Les cyberintimidations en jeux. Fête du droit 2019 : E-sport : conférences juridiques, sportives et scientifiques, Université de Bordeaux, Mar 2019, Bordeaux, France. &#x27E8;hal-02455865&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455865"},
        {
          "docid":"2455823",
          "label_s":"Marlène Dulaurans. La toma de palabra ciudadana en el web frances: \"A blogué!\". Revista de la comunicación de la SEECI, 2014, 34. &#x27E8;hal-02455823&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455823"},
        {
          "docid":"2455808",
          "label_s":"Marlène Dulaurans. Crowdfunding et médias sociaux : vers un digital plus solidaire. Gino Gramaccia. Les laboratoires du lien social : L'expérience aquitaine de la solidarité, Presses universitaires de Bordeaux, 2014, 978-2-86781-962-9. &#x27E8;hal-02455808&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455808"},
        {
          "docid":"2455810",
          "label_s":"Marlène Dulaurans, Raphael Marczak. Sites de rencontre en ligne : comment se gamifie l'amour 2.0 !. Communication & Organisation, 2019, Les « organisations collaboratives » en question, 55, pp.178. &#x27E8;10.4000/communicationorganisation.8154&#x27E9;. &#x27E8;hal-02455810&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455810"},
        {
          "docid":"2455859",
          "label_s":"Marlène Dulaurans, Jean-Christophe Fedherbe. YouTubeur, j’aime te mépriser !. Colloque international de l’ACSALF Souci, mépris, indifférence, Université de Montréal, Oct 2019, Montréal, Canada. &#x27E8;hal-02455859&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455859"},
        {
          "docid":"2455866",
          "label_s":"Marlène Dulaurans, Jean-Christophe Fedherbe. Le corps en délit : de la séduction à l'humiliation, le cyberharcèlement en question. Journée d'étude Corps, contraintes, pouvoirs, Université Bordeaux Montaigne, Mar 2019, Bordeaux, France. &#x27E8;hal-02455866&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455866"},
        {
          "docid":"2455869",
          "label_s":"Marlène Dulaurans, Oscar Motta Ramirez, Raphael Marczak. L'agilité tombée du CIEL !. Colloque international EUTIC \"Adaptabilité, flexibilité, agilité des systèmes informationnels\", Université Bordeaux Montaigne, Oct 2018, Bordeaux, France. &#x27E8;hal-02455869&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455869"},
        {
          "docid":"2455873",
          "label_s":"Marlène Dulaurans. Les réseaux sociaux mènent la danse : l’artivisme du Harlem Shake au lendemain du printemps arabe. Colloque international « La création comme résistance », Université du Québec à Montréal, Mar 2014, Montréal, France. &#x27E8;hal-02455873&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455873"},
        {
          "docid":"2455837",
          "label_s":"Marlène Dulaurans. Evelyne Broudoux, Ghislaine Chartron. BIG DATA - OPEN DATA, Quelles valeurs? Quels enjeux?, Actes du colloque \"Document numérique et société\". Revue française des sciences de l'information et de la communication, 2016, &#x27E8;10.4000/rfsic.1939&#x27E9;. &#x27E8;hal-02455837&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455837"},
        {
          "docid":"3655436",
          "label_s":"Marlène Dulaurans, Jean Christophe Fedherbe. Cyberharcèlement et communautés en ligne : les résiliences organisationnelles en jeu !. Un monde de crises au prisme des communications organisationnelles, Université Catholique de Louvain = Catholic University of Louvain [UCL], May 2022, Mons, Belgique. &#x27E8;hal-03655311&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-03655311"},
        {
          "docid":"2448532",
          "label_s":"Marlène Dulaurans. Dé-CIFRE moi ta thèse ! De l’engagement du professionnel à la distanciation du chercheur. Ballon, Justine; Le Disloquer, Pierre-Yves; Thorigny, Maxime. La recherche en action : quelles postures de recherche ? : expériences croisées de jeunes chercheurs, 2019, 978-2-37496-086-9. &#x27E8;hal-02448532&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02448532"},
        {
          "docid":"2455881",
          "label_s":"Marlène Dulaurans. \"Ensemble, c’est tout !\" : Fédérer une population autour d’un dispositif de médiation culturelle innovant. Colloque international TICEMED « TIC et construction du lien social dans la multiculturalité », Universitat de Barcelona, Jun 2011, Barcelone, France. &#x27E8;hal-02455881&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455881"},
        {
          "docid":"2455883",
          "label_s":"Marlène Dulaurans. Coopération décentralisée : d’une éthique professionnelle à une légitimité intertidale. Colloque international francophone « Éthique et métaéthique dans les professions de l'information et de la communication », Université de Béziers, Nov 2010, Béziers, France. &#x27E8;hal-02455883&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455883"},
        {
          "docid":"2455872",
          "label_s":"Hélène Marie-Montagnac, Marlène Dulaurans. Innovation numérique à l'université : la question des compétences informationnelles des étudiants en Licence. Colloque international Ludovia, Aug 2018, Ax-les-Thermes, France. &#x27E8;hal-02455872&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455872"},
        {
          "docid":"2455849",
          "label_s":"Marlène Dulaurans, Jean-Christophe Fedherbe. Analyser les discours en ligne : l'exemple du cyberharcèlement. Conférence invitée, Pôle judiciaire de la Gendarmerie Nationale, Jun 2019, Paris, France. &#x27E8;hal-02455849&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455849"},
        {
          "docid":"2448496",
          "label_s":"Marlène Dulaurans. Communication et coopération décentralisée : le cas de la région aquitaine. Sciences de l'Homme et Société. Université Bordeaux MONTAIGNE, 2012. Français. &#x27E8;NNT : &#x27E9;. &#x27E8;tel-02448496&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/tel-02448496"},
        {
          "docid":"2455829",
          "label_s":"Marlène Dulaurans, Olivia Foli. Tenir le cap épistémologique en thèse CIFRE. Les ajustements nécessaires et leurs effets sur les connaissances produites. Études de communication - Langages, information, médiations, 2013, Epistémologies, théories et pratiques professionnelles en communication des organisations, 40, &#x27E8;10.4000/edc.5118&#x27E9;. &#x27E8;hal-02455829&#x27E9;",
          "uri_s":"https://hal.science/hal-02455829"},
        {
          "docid":"2455861",
          "label_s":"Marlène Dulaurans, Jean-Christophe Fedherbe. Balance ton commentaire ! De l’injonction communautaire à la visibilité jusqu’au cyberharcèlement : le streaming en question. Colloque international La participation dans un monde de communication, UCLouvain Saint-Louis, Sep 2019, Bruxelles, Belgique. &#x27E8;hal-02455861&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455861"},
        {
          "docid":"2455855",
          "label_s":"Marlène Dulaurans. Le doctorant CIFRE, un chercheur salarié. Des rapports sociaux de production à la construction d'une posture épistémologique. Faire avec et pour : quelle posture du.de la chercheur.se dans une recherche action ? Du terrain à l'épistémologie, Université de Reims Champagne-Ardenne (URCA), Apr 2019, Reims, France. &#x27E8;hal-02455855&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455855"},
        {
          "docid":"2481615",
          "label_s":"Marlène Dulaurans, Jean-Christophe Fedherbe. De l'intimité sexuelle à l'interdit du cyberharcèlement : Enquêter sur les discours dits sensibles. Colloque R2Dip (Réseau de Recherches sur les Discours Institutionnels et Politiques), Université Bretagne Sud; (REPORTE A UNE DATE ULTERIEURE), Mar 2020, Vannes, France. &#x27E8;hal-02481615&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02481615"},
        {
          "docid":"2455846",
          "label_s":"Marlène Dulaurans, Jean-Christophe Fedherbe. E-réputation et violences en lignes. Conférence invitée, Université Paul-Valéry Montpellier 3, ITIC / Département Information-Communication, Feb 2020, Montpellier, France. &#x27E8;hal-02455846&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455846"},
        {
          "docid":"2455827",
          "label_s":"Marlène Dulaurans. Une recherche dans l’action : le cas d’une CIFRE en collectivité territoriale. Communication & Organisation, 2012, La mutation du métier de communicant public, 41, &#x27E8;10.4000/communicationorganisation.3813&#x27E9;. &#x27E8;hal-02455827&#x27E9;",
          "uri_s":"https://hal.archives-ouvertes.fr/hal-02455827"}
    ]
  }
}
```

https://api.archives-ouvertes.fr/search/?q=authIdPerson_i:1063453&fl=*

```json
{
  "response":{
    "numFound":51,
    "start":0,
    "maxScore":1.0,
    "numFoundExact":true,
    "docs":[
      {
        "docid":"2498602",
        "label_s":"Marlène Dulaurans, Jean-Christophe Fedherbe. Cyber-harassment and social networks : the CyberNeTic research project. SUNBELT 2020, Session Recent advances in Social Network Analysis, Jun 2020, Paris, France. &#x27E8;hal-02498602&#x27E9;",
        "citationRef_s":"<i>SUNBELT 2020, Session Recent advances in Social Network Analysis</i>, Jun 2020, Paris, France",
        "citationFull_s":"Marlène Dulaurans, Jean-Christophe Fedherbe. Cyber-harassment and social networks : the CyberNeTic research project. <i>SUNBELT 2020, Session Recent advances in Social Network Analysis</i>, Jun 2020, Paris, France. <a target=\"_blank\" href=\"https://hal.archives-ouvertes.fr/hal-02498602\">&#x27E8;hal-02498602&#x27E9;</a>",
        "label_bibtex":"@inproceedings{dulaurans:hal-02498602,\n  TITLE = {{Cyber-harassment and social networks : the CyberNeTic research project}},\n  AUTHOR = {Dulaurans, Marl{\\`e}ne and Fedherbe, Jean-Christophe},\n  URL = {https://hal.archives-ouvertes.fr/hal-02498602},\n  BOOKTITLE = {{SUNBELT 2020, Session Recent advances in Social Network Analysis}},\n  ADDRESS = {Paris, France},\n  YEAR = {2020},\n  MONTH = Jun,\n  KEYWORDS = {Cyberharc{\\`e}lement ; R{\\'e}seaux sociaux (Internet)},\n  HAL_ID = {hal-02498602},\n  HAL_VERSION = {v1},\n}\n",
        "label_endnote":"%0 Conference Paper\n%F Oral \n%T Cyber-harassment and social networks : the CyberNeTic research project\n%+ Médiation, Information, Communication, Art (MICA)\n%A Dulaurans, Marlène\n%A Fedherbe, Jean-Christophe\n%< avec comité de lecture\n%B SUNBELT 2020, Session Recent advances in Social Network Analysis\n%C Paris, France\n%8 2020-06-02\n%D 2020\n%K Cyberharcèlement\n%K Réseaux sociaux (Internet)\n%Z Humanities and Social SciencesConference papers\n%G English\n%L hal-02498602\n%U https://hal.archives-ouvertes.fr/hal-02498602\n%~ SHS\n%~ UNIV-BORDEAUX-MONTAIGNE\n%~ MICA\n",
        "label_coins":"<span class=\"Z3988\" title=\"ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Adc&amp;rft.type=proceedings&amp;rft.identifier=https%3A%2F%2Fhal.archives-ouvertes.fr%2Fhal-02498602&amp;rft.identifier=hal-02498602&amp;rft.title=Cyber-harassment%20and%20social%20networks%20%3A%20the%20CyberNeTic%20research%20project&amp;rft.creator=Dulaurans%2C%20Marl%C3%A8ne&amp;rft.creator=Fedherbe%2C%20Jean-Christophe&amp;rft.subject=Cyberharc%C3%A8lement&amp;rft.subject=R%C3%A9seaux%20sociaux%20%28Internet%29&amp;rft.language=en&amp;rft.date=2020-06-02&amp;rft.source=SUNBELT%202020%2C%20Session%20Recent%20advances%20in%20Social%20Network%20Analysis&amp;rft.coverage=Paris&amp;rft.coverage=France\"></span>",
        "openAccess_bool":false,
        "popularLevel_s":"0",
        "peerReviewing_s":"1",
        "invitedCommunication_s":"0",
        "audience_s":"2",
        "proceedings_s":"0",
        "domainAllCode_s":["shs"],
        "level0_domain_s":["shs"],
        "domain_s":["0.shs"],
        "fr_domainAllCodeLabel_fs":["shs_FacetSep_Sciences de l'Homme et Société"],
        "en_domainAllCodeLabel_fs":["shs_FacetSep_Humanities and Social Sciences"],
        "es_domainAllCodeLabel_fs":["shs_FacetSep_Humanities and Social Sciences"],
        "eu_domainAllCodeLabel_fs":["shs_FacetSep_domain_shs"],
        "primaryDomain_s":"shs",
        "en_title_s":["Cyber-harassment and social networks : the CyberNeTic research project"],
        "title_s":["Cyber-harassment and social networks : the CyberNeTic research project"],
        "fr_keyword_s":["Cyberharcèlement",
          "Réseaux sociaux Internet"],
        "keyword_s":["Cyberharcèlement",
          "Réseaux sociaux Internet"],
        "conferenceTitle_s":"SUNBELT 2020, Session Recent advances in Social Network Analysis",
        "conferenceStartDate_s":"2020-06-02",
        "conferenceStartDate_tdate":"2020-06-02T00:00:00Z",
        "conferenceStartDateY_i":2020,
        "conferenceStartDateM_i":6,
        "conferenceStartDateD_i":2,
        "authIdFormPerson_s":["1375936-1063453",
          "1779160-0"],
        "authIdForm_i":[1375936,
          1779160],
        "authIdPerson_i":[1063453],
        "authLastName_s":["Dulaurans",
          "Fedherbe"],
        "authFirstName_s":["Marlène",
          "Jean-Christophe"],
        "authFullName_s":["Marlène Dulaurans",
          "Jean-Christophe Fedherbe"],
        "authLastNameFirstName_s":["Dulaurans Marlène",
          "Fedherbe Jean-Christophe"],
        "authIdLastNameFirstName_fs":["1063453_FacetSep_Dulaurans Marlène",
          "0_FacetSep_Fedherbe Jean-Christophe"],
        "authFullNameIdFormPerson_fs":["Marlène Dulaurans_FacetSep_1375936-1063453",
          "Jean-Christophe Fedherbe_FacetSep_1779160-0"],
        "authAlphaLastNameFirstNameId_fs":["D_AlphaSep_Dulaurans Marlène_FacetSep_1063453",
          "F_AlphaSep_Fedherbe Jean-Christophe_FacetSep_0"],
        "authIdFullName_fs":["1063453_FacetSep_Marlène Dulaurans",
          "0_FacetSep_Jean-Christophe Fedherbe"],
        "authFullNameId_fs":["Marlène Dulaurans_FacetSep_1063453",
          "Jean-Christophe Fedherbe_FacetSep_0"],
        "authQuality_s":["aut",
          "aut"],
        "authEmailDomain_s":["hotmail.com"],
        "authIdHal_i":[1063453],
        "authFullNameFormIDPersonIDIDHal_fs":["Marlène Dulaurans_FacetSep_1375936-1063453_FacetSep_",
          "Jean-Christophe Fedherbe_FacetSep_1779160-0_FacetSep_"],
        "authFullNamePersonIDIDHal_fs":["Marlène Dulaurans_FacetSep_1063453_FacetSep_",
          "Jean-Christophe Fedherbe_FacetSep_0_FacetSep_"],
        "authIdHalFullName_fs":["_FacetSep_Marlène Dulaurans",
          "_FacetSep_Jean-Christophe Fedherbe"],
        "authFullNameIdHal_fs":["Marlène Dulaurans_FacetSep_",
          "Jean-Christophe Fedherbe_FacetSep_"],
        "authAlphaLastNameFirstNameIdHal_fs":["D_AlphaSep_Dulaurans Marlène_FacetSep_",
          "F_AlphaSep_Fedherbe Jean-Christophe_FacetSep_"],
        "authLastNameFirstNameIdHalPersonid_fs":["Dulaurans Marlène_FacetSep__FacetSep_1063453",
          "Fedherbe Jean-Christophe_FacetSep__FacetSep_0"],
        "authIdHasPrimaryStructure_fs":["1375936-1063453_FacetSep_Marlène Dulaurans_JoinSep_136101_FacetSep_Médiation, Information, Communication, Art"],
        "structPrimaryHasAuthId_fs":["136101_FacetSep_Médiation, Information, Communication, Art_JoinSep_1375936-1063453_FacetSep_Marlène Dulaurans"],
        "structPrimaryHasAuthIdHal_fs":["136101_FacetSep_Médiation, Information, Communication, Art_JoinSep__FacetSep_Dulaurans Marlène"],
        "structPrimaryHasAlphaAuthId_fs":["D_AlphaSep_136101_FacetSep_Médiation, Information, Communication, Art_JoinSep_1375936-1063453_FacetSep_Dulaurans Marlène"],
        "structPrimaryHasAlphaAuthIdHal_fs":["D_AlphaSep_136101_FacetSep_Médiation, Information, Communication, Art_JoinSep__FacetSep_Dulaurans Marlène"],
        "structPrimaryHasAlphaAuthIdHalPersonid_fs":["D_AlphaSep_136101_FacetSep_Médiation, Information, Communication, Art_JoinSep__FacetSep_1063453_FacetSep_Dulaurans Marlène"],
        "authIdHasStructure_fs":["1375936-1063453_FacetSep_Marlène Dulaurans_JoinSep_136101_FacetSep_Médiation, Information, Communication, Art",
          "1375936-1063453_FacetSep_Marlène Dulaurans_JoinSep_412629_FacetSep_Université Bordeaux Montaigne"],
        "structHasAuthId_fs":["136101_FacetSep_Médiation, Information, Communication, Art_JoinSep_1375936-1063453_FacetSep_Marlène Dulaurans",
          "412629_FacetSep_Université Bordeaux Montaigne_JoinSep_1375936-1063453_FacetSep_Marlène Dulaurans"],
        "structHasAuthIdHal_fs":["136101_FacetSep_Médiation, Information, Communication, Art_JoinSep__FacetSep_Dulaurans Marlène",
          "412629_FacetSep_Université Bordeaux Montaigne_JoinSep__FacetSep_Dulaurans Marlène"],
        "structHasAuthIdHalPersonid_s":["136101_FacetSep_Médiation, Information, Communication, Art_JoinSep__FacetSep_1063453_FacetSep_Dulaurans Marlène",
          "412629_FacetSep_Université Bordeaux Montaigne_JoinSep__FacetSep_1063453_FacetSep_Dulaurans Marlène"],
        "structHasAlphaAuthId_fs":["D_AlphaSep_136101_FacetSep_Médiation, Information, Communication, Art_JoinSep_1375936-1063453_FacetSep_Dulaurans Marlène",
          "D_AlphaSep_412629_FacetSep_Université Bordeaux Montaigne_JoinSep_1375936-1063453_FacetSep_Dulaurans Marlène"],
        "structHasAlphaAuthIdHal_fs":["D_AlphaSep_136101_FacetSep_Médiation, Information, Communication, Art_JoinSep__FacetSep_Dulaurans Marlène",
          "D_AlphaSep_412629_FacetSep_Université Bordeaux Montaigne_JoinSep__FacetSep_Dulaurans Marlène"],
        "structHasAlphaAuthIdHalPersonid_fs":["D_AlphaSep_136101_FacetSep_Médiation, Information, Communication, Art_JoinSep__FacetSep_1063453_FacetSep_Dulaurans Marlène",
          "D_AlphaSep_412629_FacetSep_Université Bordeaux Montaigne_JoinSep__FacetSep_1063453_FacetSep_Dulaurans Marlène"],
        "labStructId_i":[136101],
        "labStructIdName_fs":["136101_FacetSep_Médiation, Information, Communication, Art"],
        "labStructNameId_fs":["M_AlphaSep_Médiation, Information, Communication, Art_FacetSep_136101"],
        "labStructName_fs":["M_AlphaSep_Médiation, Information, Communication, Art"],
        "labStructAcronym_s":["MICA"],
        "labStructName_s":["Médiation, Information, Communication, Art"],
        "labStructAddress_s":["MSHA, 10 esplanade des antilles, 33607 pessac cedex"],
        "labStructCountry_s":["fr"],
        "labStructType_s":["laboratory"],
        "labStructValid_s":["VALID"],
        "labStructIdrefIdExt_s":["155971247"],
        "labStructIdrefIdExtUrl_s":["https://www.idref.fr/155971247"],
        "labStructRnsrIdExt_s":["200919217D"],
        "labStructRnsrIdExtUrl_s":["https://appliweb.dgri.education.fr/rnsr/PresenteStruct.jsp?PUBLIC=OK&numNatStruct=200919217D"],
        "structId_i":[136101,
          412629],
        "structIdName_fs":["136101_FacetSep_Médiation, Information, Communication, Art",
          "412629_FacetSep_Université Bordeaux Montaigne"],
        "structNameId_fs":["M_AlphaSep_Médiation, Information, Communication, Art_FacetSep_136101",
          "U_AlphaSep_Université Bordeaux Montaigne_FacetSep_412629"],
        "structName_fs":["M_AlphaSep_Médiation, Information, Communication, Art",
          "U_AlphaSep_Université Bordeaux Montaigne"],
        "structAcronym_s":["MICA"],
        "structName_s":["Médiation, Information, Communication, Art",
          "Université Bordeaux Montaigne"],
        "structAddress_s":["MSHA, 10 esplanade des antilles, 33607 pessac cedex"],
        "structCountry_s":["fr",
          "fr"],
        "structType_s":["laboratory",
          "institution"],
        "structValid_s":["VALID",
          "VALID"],
        "structIdrefIdExt_s":["155971247"],
        "structIdrefIdExtUrl_s":["https://www.idref.fr/155971247"],
        "structRnsrIdExt_s":["200919217D"],
        "structRnsrIdExtUrl_s":["https://appliweb.dgri.education.fr/rnsr/PresenteStruct.jsp?PUBLIC=OK&numNatStruct=200919217D"],
        "structCode_s":["UR4426",
          "UR4426"],
        "instStructId_i":[412629],
        "instStructIdName_fs":["412629_FacetSep_Université Bordeaux Montaigne"],
        "instStructNameId_fs":["U_AlphaSep_Université Bordeaux Montaigne_FacetSep_412629"],
        "instStructName_fs":["U_AlphaSep_Université Bordeaux Montaigne"],
        "instStructName_s":["Université Bordeaux Montaigne"],
        "instStructCountry_s":["fr"],
        "instStructType_s":["institution"],
        "instStructValid_s":["VALID"],
        "structIsChildOf_fs":["136101_laboratory_JoinSep_412629_FacetSep_Université Bordeaux Montaigne",
          "136101_Médiation, Information, Communication, Art_JoinSep_412629_FacetSep_Université Bordeaux Montaigne"],
        "structIsParentOf_fs":["412629_JoinSep_M_AlphaSep_136101_FacetSep_Médiation, Information, Communication, Art"],
        "structIsParentOfType_fs":["412629_JoinSep_laboratory_M_AlphaSep_136101_FacetSep_Médiation, Information, Communication, Art"],
        "labStructIsChildOf_fs":["136101_Médiation, Information, Communication, Art_JoinSep_412629_FacetSep_Université Bordeaux Montaigne"],
        "instStructIsParentOf_fs":["412629_JoinSep_M_AlphaSep_136101_FacetSep_Médiation, Information, Communication, Art"],
        "contributorId_i":667484,
        "contributorFullName_s":"Marlène Dulaurans",
        "contributorIdFullName_fs":"667484_FacetSep_Marlène Dulaurans",
        "contributorFullNameId_fs":"Marlène Dulaurans_FacetSep_667484",
        "country_s":"fr",
        "language_s":["en"],
        "halId_s":"hal-02498602",
        "uri_s":"https://hal.archives-ouvertes.fr/hal-02498602",
        "version_i":1,
        "status_i":11,
        "instance_s":"hal",
        "sid_i":1,
        "submitType_s":"notice",
        "docType_s":"COMM",
        "oldDocType_s":"COMM",
        "selfArchiving_bool":false,
        "city_s":"Paris",
        "inPress_bool":false,
        "modifiedDate_tdate":"2021-04-06T11:55:06Z",
        "modifiedDate_s":"2021-04-06 11:55:06",
        "modifiedDateY_i":2021,
        "modifiedDateM_i":4,
        "modifiedDateD_i":6,
        "submittedDate_tdate":"2020-03-04T15:38:30Z",
        "submittedDate_s":"2020-03-04 15:38:30",
        "submittedDateY_i":2020,
        "submittedDateM_i":3,
        "submittedDateD_i":4,
        "releasedDate_tdate":"2020-03-04T15:38:30Z",
        "releasedDate_s":"2020-03-04 15:38:30",
        "releasedDateY_i":2020,
        "releasedDateM_i":3,
        "releasedDateD_i":4,
        "producedDate_tdate":"2020-06-02T00:00:00Z",
        "producedDate_s":"2020-06-02",
        "producedDateY_i":2020,
        "producedDateM_i":6,
        "producedDateD_i":2,
        "publicationDate_tdate":"2020-06-02T00:00:00Z",
        "publicationDate_s":"2020-06-02",
        "publicationDateY_i":2020,
        "publicationDateM_i":6,
        "publicationDateD_i":2,
        "owners_i":[667484],
        "collId_i":[112,
          7459,
          7514,
          7459],
        "collName_s":["Sciences de l'Homme et de la Société",
          "Université Bordeaux Montaigne",
          "MICA",
          "Université Bordeaux Montaigne"],
        "collCode_s":["SHS",
          "UNIV-BORDEAUX-MONTAIGNE",
          "MICA",
          "UNIV-BORDEAUX-MONTAIGNE"],
        "collCategory_s":["AUTRE",
          "UNIV",
          "LABO",
          "UNIV"],
        "collIdName_fs":["112_FacetSep_Sciences de l'Homme et de la Société",
          "7459_FacetSep_Université Bordeaux Montaigne",
          "7514_FacetSep_MICA",
          "7459_FacetSep_Université Bordeaux Montaigne"],
        "collNameId_fs":["Sciences de l'Homme et de la Société_FacetSep_112",
          "Université Bordeaux Montaigne_FacetSep_7459",
          "MICA_FacetSep_7514",
          "Université Bordeaux Montaigne_FacetSep_7459"],
        "collCodeName_fs":["SHS_FacetSep_Sciences de l'Homme et de la Société",
          "UNIV-BORDEAUX-MONTAIGNE_FacetSep_Université Bordeaux Montaigne",
          "MICA_FacetSep_MICA",
          "UNIV-BORDEAUX-MONTAIGNE_FacetSep_Université Bordeaux Montaigne"],
        "collCategoryCodeName_fs":["AUTRE_JoinSep_SHS_FacetSep_Sciences de l'Homme et de la Société",
          "UNIV_JoinSep_UNIV-BORDEAUX-MONTAIGNE_FacetSep_Université Bordeaux Montaigne",
          "LABO_JoinSep_MICA_FacetSep_MICA",
          "UNIV_JoinSep_UNIV-BORDEAUX-MONTAIGNE_FacetSep_Université Bordeaux Montaigne"],
        "collNameCode_fs":["Sciences de l'Homme et de la Société_FacetSep_SHS",
          "Université Bordeaux Montaigne_FacetSep_UNIV-BORDEAUX-MONTAIGNE",
          "MICA_FacetSep_MICA",
          "Université Bordeaux Montaigne_FacetSep_UNIV-BORDEAUX-MONTAIGNE"],
        "collIsParentOfColl_fs":["UNIV-BORDEAUX-MONTAIGNE_FacetSep_Université Bordeaux Montaigne_JoinSep_MICA_FacetSep_MICA"],
        "collIsParentOfCategoryColl_fs":["UNIV-BORDEAUX-MONTAIGNE_FacetSep_LABO_JoinSep_MICA_FacetSep_MICA"],
        "collIsChildOfColl_fs":["MICA_FacetSep_MICA_JoinSep_UNIV-BORDEAUX-MONTAIGNE_FacetSep_Université Bordeaux Montaigne"],
        "_version_":1751459553470316544,
        "dateLastIndexed_tdate":"2022-12-06T10:20:25.540Z",
        "label_xml":"<TEI xmlns=\"http://www.tei-c.org/ns/1.0\" xmlns:xsi=\"http://www.w3.org/2001/XMLSchema-instance\" xmlns:hal=\"http://hal.archives-ouvertes.fr/\" version=\"1.1\" xsi:schemaLocation=\"http://www.tei-c.org/ns/1.0 http://api.archives-ouvertes.fr/documents/aofr-sword.xsd\">  <teiHeader>    <fileDesc>      <titleStmt>        <title>HAL TEI export of hal-02498602</title>      </titleStmt>      <publicationStmt>        <distributor>CCSD</distributor>        <availability status=\"restricted\">          <licence target=\"http://creativecommons.org/licenses/by/4.0/\">Distributed under a Creative Commons Attribution 4.0 International License</licence>        </availability>        <date when=\"2022-12-06T11:20:37+01:00\"/>      </publicationStmt>      <sourceDesc>        <p part=\"N\">HAL API platform</p>      </sourceDesc>    </fileDesc>  </teiHeader>  <text>    <body>      <listBibl>        <biblFull>          <titleStmt>            <title xml:lang=\"en\">Cyber-harassment and social networks : the CyberNeTic research project</title>            <author role=\"aut\">              <persName>                <forename type=\"first\">Marlène</forename>                <surname>Dulaurans</surname>              </persName>              <email type=\"md5\">b49e3eacda8115c85ea9ab8384e3d770</email>              <email type=\"domain\">hotmail.com</email>              <idno type=\"idhal\" notation=\"numeric\">1063453</idno>              <idno type=\"halauthorid\" notation=\"string\">1375936-1063453</idno>              <affiliation ref=\"#struct-136101\"/>            </author>            <author role=\"aut\">              <persName>                <forename type=\"first\">Jean-Christophe</forename>                <surname>Fedherbe</surname>              </persName>              <idno type=\"halauthorid\">1779160-0</idno>              <orgName ref=\"#struct-331397\"/>            </author>            <editor role=\"depositor\">              <persName>                <forename>Marlène</forename>                <surname>Dulaurans</surname>              </persName>              <email type=\"md5\">b49e3eacda8115c85ea9ab8384e3d770</email>              <email type=\"domain\">hotmail.com</email>            </editor>          </titleStmt>          <editionStmt>            <edition n=\"v1\" type=\"current\">              <date type=\"whenSubmitted\">2020-03-04 15:38:30</date>              <date type=\"whenModified\">2021-04-06 11:55:06</date>              <date type=\"whenReleased\">2020-03-04 15:38:30</date>              <date type=\"whenProduced\">2020-06-02</date>            </edition>            <respStmt>              <resp>contributor</resp>              <name key=\"667484\">                <persName>                  <forename>Marlène</forename>                  <surname>Dulaurans</surname>                </persName>                <email type=\"md5\">b49e3eacda8115c85ea9ab8384e3d770</email>                <email type=\"domain\">hotmail.com</email>              </name>            </respStmt>          </editionStmt>          <publicationStmt>            <distributor>CCSD</distributor>            <idno type=\"halId\">hal-02498602</idno>            <idno type=\"halUri\">https://hal.archives-ouvertes.fr/hal-02498602</idno>            <idno type=\"halBibtex\">dulaurans:hal-02498602</idno>            <idno type=\"halRefHtml\">&lt;i&gt;SUNBELT 2020, Session Recent advances in Social Network Analysis&lt;/i&gt;, Jun 2020, Paris, France</idno>            <idno type=\"halRef\">SUNBELT 2020, Session Recent advances in Social Network Analysis, Jun 2020, Paris, France</idno>          </publicationStmt>          <seriesStmt>            <idno type=\"stamp\" n=\"SHS\">Sciences de l'Homme et de la Société</idno>            <idno type=\"stamp\" n=\"UNIV-BORDEAUX-MONTAIGNE\">Université Bordeaux Montaigne</idno>            <idno type=\"stamp\" n=\"MICA\" corresp=\"UNIV-BORDEAUX-MONTAIGNE\">MICA</idno>          </seriesStmt>          <notesStmt>            <note type=\"audience\" n=\"2\">International</note>            <note type=\"invited\" n=\"0\">No</note>            <note type=\"popular\" n=\"0\">No</note>            <note type=\"peer\" n=\"1\">Yes</note>            <note type=\"proceedings\" n=\"0\">No</note>          </notesStmt>          <sourceDesc>            <biblStruct>              <analytic>                <title xml:lang=\"en\">Cyber-harassment and social networks : the CyberNeTic research project</title>                <author role=\"aut\">                  <persName>                    <forename type=\"first\">Marlène</forename>                    <surname>Dulaurans</surname>                  </persName>                  <email type=\"md5\">b49e3eacda8115c85ea9ab8384e3d770</email>                  <email type=\"domain\">hotmail.com</email>                  <idno type=\"idhal\" notation=\"numeric\">1063453</idno>                  <idno type=\"halauthorid\" notation=\"string\">1375936-1063453</idno>                  <affiliation ref=\"#struct-136101\"/>                </author>                <author role=\"aut\">                  <persName>                    <forename type=\"first\">Jean-Christophe</forename>                    <surname>Fedherbe</surname>                  </persName>                  <idno type=\"halauthorid\">1779160-0</idno>                  <orgName ref=\"#struct-331397\"/>                </author>              </analytic>              <monogr>                <meeting>                  <title>SUNBELT 2020, Session Recent advances in Social Network Analysis</title>                  <date type=\"start\">2020-06-02</date>                  <settlement>Paris</settlement>                  <country key=\"FR\">France</country>                </meeting>                <imprint/>              </monogr>            </biblStruct>          </sourceDesc>          <profileDesc>            <langUsage>              <language ident=\"en\">English</language>            </langUsage>            <textClass>              <keywords scheme=\"author\">                <term xml:lang=\"fr\">Cyberharcèlement</term>                <term xml:lang=\"fr\">Réseaux sociaux (Internet)</term>              </keywords>              <classCode scheme=\"halDomain\" n=\"shs\">Humanities and Social Sciences</classCode>              <classCode scheme=\"halTypology\" n=\"COMM\">Conference papers</classCode>              <classCode scheme=\"halOldTypology\" n=\"COMM\">Conference papers</classCode>              <classCode scheme=\"halTreeTypology\" n=\"COMM\">Conference papers</classCode>            </textClass>          </profileDesc>        </biblFull>      </listBibl>    </body>    <back>      <listOrg type=\"structures\">        <org type=\"laboratory\" xml:id=\"struct-136101\" status=\"VALID\">          <idno type=\"IdRef\">155971247</idno>          <idno type=\"RNSR\">200919217D</idno>          <orgName>Médiation, Information, Communication, Art</orgName>          <orgName type=\"acronym\">MICA</orgName>          <desc>            <address>              <addrLine>MSHA, 10 esplanade des antilles, 33607 pessac cedex</addrLine>              <country key=\"FR\"/>            </address>            <ref type=\"url\">http://mica.u-bordeaux3.fr</ref>          </desc>          <listRelation>            <relation name=\"UR4426\" active=\"#struct-412629\" type=\"direct\"/>          </listRelation>        </org>        <org type=\"institution\" xml:id=\"struct-412629\" status=\"VALID\">          <orgName>Université Bordeaux Montaigne</orgName>          <date type=\"start\">2014-01-01</date>          <desc>            <address>              <country key=\"FR\"/>            </address>            <ref type=\"url\">http://www.u-bordeaux-montaigne.fr/</ref>          </desc>        </org>      </listOrg>    </back>  </text></TEI>"
      },
      ...
    ]
  }
}
```

https://api.archives-ouvertes.fr/search/?q=authIdHal_s:arnaudlevy

```json
{
  "response":{
    "numFound":1,
    "start":0,
    "maxScore":6.30194,
    "numFoundExact":true,
    "docs":[
      {
        "docid":"2455856",
        "label_s":"Marlène Dulaurans, Arnaud Levy. Université, pédagogie, innovation : cherchez l'intrus !. Pratiques de la communication, 2019, 1, pp.39-59. &#x27E8;hal-02455856&#x27E9;",
        "uri_s":"https://hal.science/hal-02455856"}
    ]
  }
}
``` 