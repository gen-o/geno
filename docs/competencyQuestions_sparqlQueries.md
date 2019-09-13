
## Competency questions


| Identifier | 1 |
| ------------- |:-------------:|
| Question | Which works by Roud are rooted in his diary? |
| Outcome | The list of all the works whose genesis includes a diary entry |


| Identifier | 2 |
| ------------- |:-------------:|
| Question | How many avant-textes are included in the genetic dossiers of Roud's works? |
| Outcome | The number of avant-textes included in each genetic dossier |



| Identifier | 3 |
| ------------- |:-------------:|
| Question | In which works it is reused the article "Cendre" published in the periodical Aujourd'hui in 1930? |
| Outcome | The list of all the works that have "Cendre" as one of their sources |



| Identifier | 4 |
| ------------- |:-------------:|
| Question | To how many genetic dossiers an avant-texte can belong to in Roud's works? |
| Outcome | The maximum number of connections between an avant-texte and a genetic dossier |




## Corresponding sparql queries

**Question 1**

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX onto: <http://example.org/geneticOntology/onto#>
select ?Publication where { 
	?GeneticDossier rdf:type onto:GeneticDossier .
    ?Publication rdf:type onto:Publication .
    ?GeneticDossier onto:geneticDossierResultsInPublication ?Publication .
    ?Diary rdf:type onto:Diary .
    ?Diary onto:diaryIsReusedInGeneticDossier ?GeneticDossier .
}
group by ?Publication
```

**Question 2**

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX onto: <http://example.org/geneticOntology/onto#>
select ?GeneticDossier (count(?AvantTexte) as ?count)
where { 
	?AvantTexte rdf:type onto:AvantTexte .
    ?GeneticDossier rdf:type onto:GeneticDossier .
    ?AvantTexte onto:AvantTexteIsPartOfGeneticDossier ?GeneticDossier .
}
group by ?GeneticDossier
```

**Question 3**

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX onto: <http://example.org/geneticOntology/onto#>
PREFIX data: <http://example.org/geneticOntology/data#>
select ?Publication
where { 
	?Publication rdf:type onto:Publication .
    ?GeneticDossier rdf:type onto:GeneticDossier .
    ?GeneticDossier onto:geneticDossierResultsInPublication ?Publication .
    data:Cendre_Ajourdhui_19300710 ?PublicationIsReusedInGeneticDossier ?GeneticDossier .
} limit 100 
```

**Question 4**

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX onto: <http://example.org/geneticOntology/onto#>
select ?AvantTexte (count(?GeneticDossier) as ?count)
where { 
	?AvantTexte rdf:type onto:AvantTexte .
    ?GeneticDossier rdf:type onto:GeneticDossier .
    ?AvantTexte onto:avantTexteIsPartOfGeneticDossier ?GeneticDossier .
}
group by ?AvantTexte
order by desc(?count)
```