---
title: Règles d'écriture
description: >
  Connaitre les styles de nommages et les conventions d'écriture du code
---
# Écriture du code
## Go + HTML 
- Indentation = 2 espaces
- Espaces entre les chevrons

### Exemple
```
{{ if something }}
  something
{{ end }}

{{- if something -}}
  <div> something </div>
{{- end -}}
```

## SASS
- Indentation = 4 espaces
- Propriétés rangées par ordre alphabétique

### Exemple
```
body
    background: white
    text-align: left
```

## JS
- Indentation = 4 espaces
- Coder en ES5
- Point-virgule obligatoire en fin de ligne
- Espaces entre les commandes et les parenthèses
```
if (true) {
    return;
}
```

# Nommages et convention Git
## Pull Request
- En français
- Un nom court mais clair
- Les commits sont squash dans les PR

## Branche
- En anglais
- En kebab-case: block-truc-fix-bug

## Commit
- En anglais
- À minima deux trois mots décrivant la modification 


