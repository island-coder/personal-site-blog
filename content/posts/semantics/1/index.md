---
title: Constructing a Knowledge Graph on Sri Lankan Politics
seo_title: Constructing a Knowledge Graph on Sri Lankan Politics
summary: A guide on practical application of SPARQL querying to generate custom RDF data.
description: A guide on practical application of SPARQL querying to generate custom RDF data.
slug: 1
author: Vihanga Marasinghe

draft: false
date: 2024-05-27T12:56:42+05:30
lastmod: 
expiryDate: 
publishDate: 

feature_image: 
feature_image_alt: 

categories:
tags:
    - knowledge graphs
series: 
    - Semantic Representation

toc: true
related: true
social_share: true
newsletter: false
disable_comments: false
---


##  Introduction

In this article, I aim to demonstrate how we can query data from a public knowledge base and create a knowledge graph for a particular domain. The main goal is to extract data related to Sri Lankan politics using Wikidata. Before we dive into the technical details, let's first get an understanding of some key concepts: knowledge graphs, Wikidata, RDF and SPARQL.

### Knowledge Graphs 

A knowledge graph is a network of real-world entities and their interrelations, organized in a graph structure. This allows for the integration of diverse data sources, providing a unified framework to connect various pieces of information. Knowledge graphs are powerful tools for data analysis, enabling complex queries and insightful visualizations.


  {{< figure src="kg.png" attr="Fuzheado, CC BY-SA 4.0 <https://creativecommons.org/licenses/by-sa/4.0>, via Wikimedia Commons">}}

  

### Wikidata 

Wikidata is a collaboratively edited knowledge base hosted by the Wikimedia Foundation. It acts as a central repository of structured data for Wikipedia, Wikivoyage, Wikisource, and other Wikimedia projects. Wikidata provides a rich source of interconnected data on a wide array of subjects, including historical events, scientific concepts, and political figures.

### RDF (Resource Description Framework)

The Resource Description Framework (RDF) is a standard model for data interchange on the web. RDF allows data to be linked across different sources, making it possible to merge information from various origins. The basic structure of RDF data consists of triples: subject, predicate, and object. This structure is ideal for representing knowledge graphs.

### SPARQL (SPARQL Protocol and RDF Query Language)

SPARQL is the query language for RDF. It allows us to extract information from RDF graphs by specifying patterns to match against the data. SPARQL queries can be used to retrieve and manipulate data stored in RDF format, making it a powerful tool for querying knowledge graphs.

## Querying Wikidata Using SPARQL

In this section, we will define SPARQL queries to extract data related to Sri Lankan politics from Wikidata. We will cover querying for politicians, political parties and coalitions, and political offices or positions. These queries will be constructed using Python multiline strings and the results will be formatted as RDF triples.

### Query Prefixes

First, we define the necessary query prefixes. These include standard namespaces and a custom namespace (slpg) for our schema.

``` python
query_prefixes="""
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX slpg: <http://www.slpg.lk/>
"""
```
### Query for Political figures

The following query extracts information about Sri Lankan politicians. It constructs RDF triples for each politician with attributes such as name, date of birth, political party, positions held, education, religion, spouse, place of birth, siblings, and children.

``` python
query_persons = query_prefixes + """
CONSTRUCT {
    ?person
    slpg:label ?personLabel;
    slpg:dateOfBirth ?dob;
    slpg:memberOf ?party;
    slpg:holds ?position;
    slpg:educatedAt ?educated;
    slpg:religion ?religion;
    slpg:spouseOf ?spouse;
    slpg:placeOfBirth ?placeOfBirth;
    slpg:siblingOf ?sibling;
    slpg:parentOf ?children;
    a slpg:PERSON.
}
WHERE {
  ?person wdt:P31 wd:Q5;
          wdt:P27 wd:Q854;
          wdt:P106 wd:Q82955;
  wdt:P569 ?dob.
  OPTIONAL{
    ?person wdt:P102 ?party
  }
  OPTIONAL{
    ?person wdt:P39 ?position
  }
  OPTIONAL{
    ?person wdt:P69 ?educated
  }
  OPTIONAL{
     ?person wdt:P140 ?religion;
  }
  OPTIONAL{
     ?person wdt:P26 ?spouse;
  }
  OPTIONAL{
     ?person wdt:P19 ?placeOfBirth;
  }
  OPTIONAL{
     ?person wdt:P3373 ?sibling;
  }
  OPTIONAL{
     ?person wdt:P40 ?children;
  }
  ?person rdfs:label ?personLabel . FILTER (lang(?personLabel) = "en")
  wd:Q5 rdfs:label ?humanLabel . FILTER (lang(?humanLabel) = "en")
}
"""
```
This query can be tested on the [Wikidata SPARQL Query Service ](https://query.wikidata.org/https://query.wikidata.org/E)

### Query for Parties and Coalitions

The following query extracts information about political parties and coalitions. It constructs RDF triples for each party with attributes such as name, ideology, chairperson, founding date, founder, and political alignment.

``` python 
query_parties = query_prefixes + """
CONSTRUCT {
  ?party  slpg:label ?partyLabel;
          slpg:hasIdeology ?ideology;
          slpg:hasChairperson ?leader;
          slpg:foundOn ?foundingDate;
          slpg:foundBy ?founder;
          slpg:hasPoliticalAlignment ?politicalAlignment;
          a slpg:PARTY_OR_COALITION;
}
WHERE {
  ?person wdt:P31 wd:Q5;
         wdt:P27 wd:Q854;
         wdt:P106 wd:Q82955;
         wdt:P102 ?party.
  OPTIONAL { ?party wdt:P1142  ?ideology. }  # Ideology
  OPTIONAL { ?party wdt:P488 ?leader. }  # Leader
  OPTIONAL { ?party wdt:P112 ?founder. }  # Founder
  OPTIONAL { ?party wdt:P1387 ?politicalAlignment. }  # Founder
  OPTIONAL { ?party wdt:P571 ?foundingDate. }  # Founding date
  ?party rdfs:label ?partyLabel.FILTER(langMatches( lang(?partyLabel), "EN" ) )
}
"""
```

### Additional Queries

Similarly, you can create queries for other relevant data such as poltical offices,educational institutions, religious beliefs, family members, political ideologies, and locations. These queries follow the same structure as above, adjusting the CONSTRUCT and WHERE clauses to match the specific data you are interested in.

## Performing the queries 

Next, we use the library SPARQLWrapper to execute these queries against the Wikidata SPARQL endpoint, convert the results into RDF graphs, and then merge these graphs. The final graph is serialized into turtle format and saved.

``` python
from SPARQLWrapper import SPARQLWrapper
endpoint_url = "https://query.wikidata.org/sparql"
sparql = SPARQLWrapper(endpoint_url)
queries=[query_persons,query_parties,query_offices,query_education,query_religion,query_family,query_ideology,query_location]
graphs = []
for query in queries:
    sparql.setQuery(query)
    graph = sparql.queryAndConvert()   # returns rdflib Graph objects ,rdflib is a powerful library used to handle rdf data in python
    graphs.append(graph)
merged_graph = Graph()
for g in graphs:
    merged_graph += g
merged_graph.serialize(destination='merged_graph.ttl', format='turtle')
```

## Conclusion

In this post we looked at how we can query a public knowledge base to generate RDF data.In the next sections we aim to injest this data for analytics and visualization.