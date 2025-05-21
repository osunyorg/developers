---
title: Documents ?
weight: 9
---

Est-ce vraiment dans ce namespace ? Peut-être un DAM ?

1 document peut concerner :
- toute l'université (charte numérique)
- 1 école (règlement intérieur de l'école)
- 1 campus (règlement intérieur du campus)
- 1 formation (contrat d'alternance type)
- 1 session (calendrier pédagogique de l'année en cours)

Attributes:
- university:references
- name:string
- school:references (optional)
- campus:references (optional)
- program:references (optional)
- session:references (optional)
