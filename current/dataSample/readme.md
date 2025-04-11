
# Data samples and competency questions

This readme contains competency questions that have been used for the ontology conception and development, as well as their corresponding SPARQL queries.

Two data samples are also provided, to exemplify the capabilities of the ontology and to test the queries: [https://doi.org/10.5281/zenodo.8287801](https://doi.org/10.5281/zenodo.8287801).

To run the queries, use your preferred triplestore or [this notebook](runSPARQL.ipynb). For an interactive version of the notebook, launch myBinder by clicking on the button (might take several minutes).

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/gen-o/geno/HEAD?labpath=current%2FdataSample%2FrunSPARQL.ipynb)



## Competency questions

The questions apply to the Roud dataset, but most of them would work also on other datasets.


| 1 |      |
|-----|-------|
| Question | Which publications by Roud are rooted in his diary? |
| Outcome | The list of all the publications whose genesis includes a diary entry |


| 2 |      |
| ------------- |-------------|
| Question | In which publications it is reused the article "Cendre", originally published in the periodical Aujourd'hui in 1930? |
| Outcome | The list of all the publications that have "Cendre" as one of their sources |



| 3 |      |
| ------------- |-------------|
| Question | To how many genetic dossiers a genetic witness can belong to in Roud's works? |
| Outcome | The maximum number of connections between a witness and a genetic dossier |



| 4 |      |
| ------------- |-------------|
| Question | List everything that belongs to the dossier of Feuillets (1929) |
| Outcome | Retrieve all the genetic witnesses and the resources (publications, publication parts, dossiers) reused in the dossier of Feuillets (1929) |


| 5 |      |
| ------------- |-------------|
| Question | List all witnesses belonging to a pre-compositional phase in Roud's corpus |
| Outcome | Retrieve all resources of type pre-compositional, inferred as equivalent to the union of plans, lists and scenarios. |

| 6 |      |
| ------------- |-------------|
| Question | How many genetic witnesses belong to a genetic dossier of Roud's publications? |
| Outcome | The number of genetic witnesses included in each dossier. If a publication has multiple dossiers for the different parts, they are counted separately |





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

Simplified version that works out of the box in the notebook (without parsing the results):
```sparql
prefix geno: <https://w3id.org/geno#>
PREFIX roud-oeuvres: <http://www.knora.org/ontology/0112/roud-oeuvres#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
select ?Publication where { 	
    ?Diary rdf:type geno:DiaryEntry .
    ?Diary geno:isMemberOfDossier ?GeneticDossier .    
    {?GeneticDossier geno:resultsInPublication ?Publication .}
    UNION
    {?GeneticDossier geno:resultsInPublicationPart ?PubPart .
    ?PubPart geno:isPartOfPublication ?Publication .} 
}
```


**Question 2**

```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX geno: <https://w3id.org/geno#>
PREFIX roud-oeuvres: <http://www.knora.org/ontology/0112/roud-oeuvres#>
select ?GeneticDossier ?PubTitle ?PubPartTitle ?PubDate
where { 
    <https://ark.dasch.swiss/ark:/72163/1/0112/w77KbuWKQUe8=8au2llnpQq> geno:publicationIsReusedInDossier ?GeneticDossier .
    
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

Simplified version that works out of the box in the notebook (without parsing the results):
```sparql
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX geno: <https://w3id.org/geno#>
PREFIX roud-oeuvres: <http://www.knora.org/ontology/0112/roud-oeuvres#>
select ?Publication
where { 
    <https://ark.dasch.swiss/ark:/72163/1/0112/w77KbuWKQUe8=8au2llnpQq> geno:publicationIsReusedInDossier ?GeneticDossier .
    ?GeneticDossier geno:resultsInPublicationPart ?Publication .
}
```

**Question 3**

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

**Question 4**

```sparql
prefix geno: <https://w3id.org/geno#>
select ?x where { 
    BIND(<https://ark.dasch.swiss/ark:/72163/1/0112/a42Rpz7FRFinjNZ6ny4Rig2> as ?FeuilletsDossier)
    {?x geno:isMemberOfDossier ?FeuilletsDossier . }
    UNION
    {?x geno:isReusedInDossier ?FeuilletsDossier . }   
}
```

**Question 5**

```sparql
prefix geno: <https://w3id.org/geno#>
PREFIX roud-oeuvres: <http://www.knora.org/ontology/0112/roud-oeuvres#>
select * where { 
	?s a geno:PrecompositionalPhaseResource .
	?s roud-oeuvres:manuscriptHasTitle ?title .
	?s geno:hasGeneticStage ?stage .
} 
```

Simplified version that works out of the box in the notebook (without parsing the results):
```sparql
prefix geno: <https://w3id.org/geno#>
PREFIX roud-oeuvres: <http://www.knora.org/ontology/0112/roud-oeuvres#>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
select ?s where { 
	?s rdf:type geno:PrecompositionalPhaseResource .
} 
```


**Question 6**

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

