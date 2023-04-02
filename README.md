# GENO, a Genetic Ontology

**Website**: https://gen-o.github.io

**URL**: http://purl.org/hum/geno

**Ontology source**: http://purl.org/hum/geno.ttl

This is the development repo of GENO, an OWL2 ontology for the field of genetic criticism and genetic editing.

For a **human-readable** version of the latest version of GENO, [visualize with LODE](http://purl.org/hum/geno).


## Repo structure

This repo contains

- `onto`. A folder with the ontology (and previous versions for documentation)
- `data`. A folder where are collected data samples for testing purpose
- `docs`. A folder containing documentation about the ontology, including the motivating scenario and some competency questions with related sparql queries.

<!--
## Work in progress

Before continuing the development, we want to hear from the community. If you want to participate or to receive updates, follow us here or write us at elena.spadini@unil.ch and alessio.christen@unil.ch.
-->

## GENO implementation for Gustave Roud

GENO has been created as part of the project « Gustave Roud. *Œuvres complètes* », the critical edition of the complete works of the Swiss author Gustave Roud (1897-1976) (directed by D. Maggetti and C. Jaquier, University of Lausanne, 2017-2021). In this context, it is implemented in the DaSCH platform [DSP](https://www.dasch.swiss/), the software framework used in the edition. In DSP, each project has its own ontology, which should use [Knora Base](https://docs.dasch.swiss/2023.03.02/DSP-API/02-dsp-ontologies/knora-base/) as upper-level ontology. 

To know more about the project and the implementation of GENO in it, see [Gustave Roud. Textes et archives](https://roud.unil.ch/).


## GENO in context

GENO is meant to be general enough to accommodate the needs of different projects of scholarly editing and genetic criticism. In practice, it should be combined with other ontologies for describing bibliographic and archival resources. GENO is quite small and simple, because it only focuses on the materials from the point of view of genetic criticism.



## Test (e.g. with GraphDB)

1. download or clone the repo
2. create a repository in the DB
2. import from rdf:
	- select the ontology (.ttl), upload, when asked insert as Base URI and as Context http://purl.org/hum/geno/
	- select the data sample (.ttl), upload, when asked insert as Base URI and as Context http://purl.org/hum/geno/data/ (by uploading the ontology and the data in two different graphs, you can update or delete one without touching the other)
3. Now you can explore and query the data. Examples of sparql queries are available [here](docs/competencyQuestions_sparqlQueries.md).



