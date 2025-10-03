---
title: Maintenabilité
---

## Problématique

Comment éviter de devoir repasser sur tous les sites développés lorsqu'on modifie un partiel d'item qui serait overridé ?

## Cas concret

Un partiel d'item, par exemple : `partials/jobs/partials/job.html`, cet item contient le titre et le sous-titre d'une offre d'emploi. Un site quelconque override ce partiel pour ajouter une information supplémentaire à côté du titre. Par la suite on ajoute au partiel d'origine (dans le tèeme) l'affichage des catégories nouvellement implémenté. Il faudra alors mettre à jour manuellement l'override du site en question. Il faut répeter l'opération pour chaque site concerné.

```html {filename="partials/jobs/partials/job.html"}
<article itemscope itemtype="https://schema.org/JobPosting">
  <div class="job-content">
    <h3>{{ .Title }}</h3>
    <p>{{ .Params.summary }}</p>
  </div>
</article>
```

## Situation idéale

En suivant l'exemple précédent, si on ajoute les catégories à l'offre d'emploi, il ne faudrait pas repasser sur tous les sites qui l'overrident.

## Solution proposée

Mise en place de partiels intermédiaires, des `fragments`, qui vont permettre de découper en plus petits morceaux les éléments à afficher.

Ils se rangent dans `partials/jobs/fragments/job/`. On va créer un `heading` qui aura pour charge d'afficher le titre. 

Pour modifier le titre du `job` il suffit alors d'overrider le passe-plat `partials/jobs/partials/job/heading.html`.

```html{filename="partials/jobs/partials/job.html"}
<article itemscope itemtype="https://schema.org/JobPosting">
  <div class="job-content">
    {{ partial "jobs/partials/job/heading.html" }}
    {{ partial "jobs/partials/job/summary.html" }}
  </div>
</article>
```

```html{filename="partials/jobs/partials/job/heading.html"}
<h3>{{ .Title }}</h3>
```
