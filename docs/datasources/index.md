# Overview

The list of Datafeeds, and their details is [managed in OpenProject](https://openproject.nfdi4culture.de/projects/culture-knowledge-graph/work_packages?query_id=1773)

## Defined Datafeeds

|                                  |                                                                                    |
| -------------------------------- | ---------------------------------------------------------------------------------- |
| https://nfdi4culture.de/id/E6304 | Metadata on audio objects from APSearch                                            |
| https://nfdi4culture.de/id/E6569 | Inscriptions metadata from Deutsche Inschriften Online/Epigraf                     |
| https://nfdi4culture.de/id/E6317 | Headstone inscriptions metadata from epidat                                        |
| https://nfdi4culture.de/id/E6635 | Metadata on Architectural Drawings of the Collection Albrecht Haupt (Gesah)        |
| https://nfdi4culture.de/id/E5320 | Musical event metadata from musiconn.performance                                   |
| https://nfdi4culture.de/id/E6078 | Metadata of 3D models in Semantic Kompakkt                                         |
| https://nfdi4culture.de/id/E6079 | Metadata of 3D models in Kompakkt Repository                                       |
| https://nfdi4culture.de/id/E5305 | Metadata of works in the repertoire of the Detmolder Hoftheater 1825–1875          |
| https://nfdi4culture.de/id/E5307 | Culture Portal Research Information Metadata                                       |
| https://nfdi4culture.de/id/E5308 | Image Metadata from the Corpus Vitrearum Germany                                   |
| https://nfdi4culture.de/id/E5313 | Musical Sources in the RISM Online Database                                        |
| https://nfdi4culture.de/id/E5313 | Musical Sources in the RISM Online Database                                        |
| https://nfdi4culture.de/id/E5330 | Image datasets of the lexicon of revolutionary iconography 1789-1889 in prometheus |
| https://nfdi4culture.de/id/E5377 | Libretti in the musicological library of the DHI Rom                               |
| https://nfdi4culture.de/id/E5378 | Letters from the Digital Edition Ferdinand Gregorovius                             |
| https://nfdi4culture.de/id/E5985 | Metadata from the Text+ Registry                                                   |
| https://nfdi4culture.de/id/E6064 | Image metadata from the photographic collection of the Bibliotheca Hertziana       |
| https://nfdi4culture.de/id/E6077 | Metadata from the Corpus of Baroque Ceiling Painting in Germany                    |
| https://nfdi4culture.de/id/E6113 | Musical works metadata from the Telemann catalog raisonné                          |
| https://nfdi4culture.de/id/E6155 | Musical Works in the Digital Catalogue Raisonné of the Gluck Gesamtausgabe         |
| https://nfdi4culture.de/id/E6161 | Metadata from the Bildindex der Kunst und Architektur                              |
| https://nfdi4culture.de/id/E6167 | Sources from the Digital Edition of Der STURM                                      |
| https://nfdi4culture.de/id/E6290 | [Object metadata from heidICON](E6290.md)                                          |
| https://nfdi4culture.de/id/E6349 | Newspaper metadata of the German Newspaper Portal                                  |
| https://nfdi4culture.de/id/E6712 | Regesta Metadata from Regista Imperii                                              |
| https://nfdi4culture.de/id/E6815 | Metadata on visual art works from duerer.online                                    |
| https://nfdi4culture.de/id/E6817 | Metadata on creative works from museum.digital:global                              |

## Exploring the Knowledge Graph

How do we get a list of the datafeeds, from looking at data in the KG?

We can try doing some data queries, but that proves tricky of you do not really know (or can remember) the shape of the data.

Picking a random resource and looking at its properties is a potential start, like this:

```sparql
SELECT *
WHERE {
  <http://epigraf.inschriften.net/iri/articles/dio-article/dio~di085-0136#dfi> ?pred ?obj .
}
LIMIT 10
```

but then you still get stuck as the OBO-style properties are not so clear. And as the T-Box is not loaded, we can't really do this either:

```sparql
SELECT *
WHERE {
  <https://nfdi4culture.de/ontology/CTO_0001026> ?pred ?obj .
}
LIMIT 10
```

But by looking at the [ontology]('../ontology.md#core-cto-classes') we can find some information on the data feed integration,
So we can try and do more, for example get the datafeeds and their labels:

```shmarql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>

SELECT ?s ?label
WHERE {
  ?s ?p schema:DataFeed ;
     rdfs:label ?label .
}
```

Watch out! If you try and do this, you will get zero results because of the http vs https scheme for Schema

```sparql hl_lines="1" title="Schema prefix with https"
PREFIX schema: <https://schema.org/>

SELECT *
WHERE {
?s ?p schema:DataFeed .
}
```

We can see how many items are in a datafeed with:

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>

SELECT (COUNT(?o) AS ?co)
WHERE {
  <https://nfdi4culture.de/id/E6304> schema:dataFeedElement ?o .
}
```
