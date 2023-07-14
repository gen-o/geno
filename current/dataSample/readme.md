
# Data samples and examples of queries

A brief description of the data is contained in each file.


## Questions

**These questions apply to the Roud data sample.**


| 1 |      |
|-----|-------|
| Question | Which publications by Roud are rooted in his diary? |
| Outcome | The list of all the publications whose genesis includes a diary entry |


| 2 |      |
| ------------- |-------------|
| Question | How many genetic witnesses belong to a genetic dossier of Roud's publications? |
| Outcome | The number of genetic witnesses included in each dossier. If a publication has multiple dossiers for the different parts, they are counted separately |



| 3 |      |
| ------------- |-------------|
| Question | In which publications it is reused the article "Cendre", originally published in the periodical Aujourd'hui in 1930? |
| Outcome | The list of all the publications that have "Cendre" as one of their sources |



| 4 |      |
| ------------- |-------------|
| Question | To how many genetic dossiers a genetic witness can belong to in Roud's works? |
| Outcome | The maximum number of connections between a witness and a genetic dossier |



| 5 |      |
| ------------- |-------------|
| Question | List everything that belongs to the dossier of Feuillets (1929) |
| Outcome | Retrieve all the genetic witnesses and the resources (publications, publication parts, dossiers) reused in the dossier of Feuillets (1929) |


| 6 |      |
| ------------- |-------------|
| Question | All witnesses belonging to a pre-compositional phase in Roud's corpus |
| Outcome | Retrieve all plans, lists and scenarios. |





## Corresponding sparql queries

**Question 1**

```sparql
prefix geno: <https://w3id.org/geno#>
PREFIX roud-oeuvres: <http://www.knora.org/ontology/0112/roud-oeuvres#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
select ?Publication ?PubTitle ?PubDate where { 
	
    ?Diary rdf:type geno:DiaryEntry .
    ?Diary geno:isMemberOfDossier ?GeneticDossier .
    
    {?GeneticDossier geno:resultsInPublication ?Publication .}
    UNION
    {?GeneticDossier geno:resultsInPublicationPart ?PubPart .
    ?PubPart geno:isPartOfPublication ?Publication .}
    
    # show the publication title and date
    ?Publication roud-oeuvres:publicationHasTitle ?PubTitle .
    ?Publication roud-oeuvres:publicationHasDate ?PubDate .
    
}
GROUP BY ?Publication ?PubTitle ?PubDate
ORDER BY ?PubTitle ?PubDate
```

**Question 2**

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX geno: <https://w3id.org/geno#>
PREFIX roud-oeuvres: <http://www.knora.org/ontology/0112/roud-oeuvres#>
select ?GeneticDossier (count(?Witness) as ?count) ?PubTitle ?PubPartTitle ?PubDate
where { 
    ?Witness geno:isMemberOfDossier ?GeneticDossier .
    
    # show the corresponding publication and publication part title
    {?GeneticDossier geno:resultsInPublication ?Publication .}
    UNION
    {?GeneticDossier geno:resultsInPublicationPart ?PubPart .
    ?PubPart roud-oeuvres:pubPartHasTitle ?PubPartTitle .
    ?PubPart geno:isPartOfPublication ?Publication .}    
    ?Publication roud-oeuvres:publicationHasTitle ?PubTitle .
    ?Publication roud-oeuvres:publicationHasDate ?PubDate .
}
group by ?GeneticDossier ?PubTitle ?PubPartTitle ?PubDate
```

**Question 3**

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX geno: <https://w3id.org/geno#>
PREFIX roud-oeuvres: <http://www.knora.org/ontology/0112/roud-oeuvres#>
select ?GeneticDossier ?PubTitle ?PubPartTitle ?PubDate
where { 
    <http://rdfh.ch/0112/w77KbuWKQUe8-8au2llnpQ> geno:publicationIsReusedInDossier ?GeneticDossier .
    
    # show the corresponding publication and publication part title
    {?GeneticDossier geno:resultsInPublication ?Publication .}
    UNION
    {?GeneticDossier geno:resultsInPublicationPart ?PubPart .
    ?PubPart roud-oeuvres:pubPartHasTitle ?PubPartTitle .
    ?PubPart geno:isPartOfPublication ?Publication .}    
    ?Publication roud-oeuvres:publicationHasTitle ?PubTitle .
    ?Publication roud-oeuvres:publicationHasDate ?PubDate .
}
group by ?GeneticDossier ?PubTitle ?PubPartTitle ?PubDate
```

**Question 4**

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX geno: <https://w3id.org/geno#>
PREFIX roud-oeuvres: <http://www.knora.org/ontology/0112/roud-oeuvres#>
select ?Witness (count(?GeneticDossier) as ?count)
where { 
	?Witness geno:isMemberOfDossier ?GeneticDossier .
}
group by ?Witness
order by desc(?count)
```

**Question 5**

```sparql
prefix geno: <https://w3id.org/geno#>
select ?x where { 
    BIND(<http://rdfh.ch/0112/a42Rpz7FRFinjNZ6ny4Rig> as ?FeuilletsDossier)
    {?x geno:isMemberOfDossier ?FeuilletsDossier . }
    UNION
    {?x geno:isReusedInDossier ?FeuilletsDossier . }   
}
```

**Question 6**

```sparql
prefix geno: <https://w3id.org/geno#>
PREFIX roud-oeuvres: <http://www.knora.org/ontology/0112/roud-oeuvres#>
select * where { 
	?s a geno:PrecompositionalPhaseResource .
	?s roud-oeuvres:manuscriptHasTitle ?title .
	?s geno:hasGeneticStage ?stage .
} 
```