# Overview

The list of Datafeeds, and their details is [managed in OpenProject](https://openproject.nfdi4culture.de/projects/culture-knowledge-graph/work_packages?query_id=1773)

## Defined Datafeeds

Get the datafeeds and their labels from the KG:

```shmarql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>

SELECT ?nfdi4cID ?label
WHERE {
  ?nfdi4cID ?p schema:DataFeed ;
     rdfs:label ?label .
}
ORDER BY ?nfdi4cID
```

Note how some items are appearing more than once, so possibly they have multiple triples. (eg. https://nfdi4culture.de/id/E5313 ) As an aside, we do not currenlty use named graphs in the KG, it is worth investigating this as well.

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

But by looking at the [ontology](../ontology.md#core-cto-classes) we can find some information on the data feed integration,

!!! Careful

    Watch out! If you try and query for schema.org properties like this example, you will get zero results because of the http vs https scheme for Schema.org

    ```sparql hl_lines="1" title="Schema prefix with https"
    PREFIX schema: <https://schema.org/>

    SELECT *
    WHERE {
    ?s ?p schema:DataFeed .
    }
    # Gives zero results!
    ```

We can see how many items are in a datafeed with:

```shmarql
# shmarql-view: piechart
# shmarql-names: name
# shmarql-values: elements
# shmarql-label: Instance Count

PREFIX rdfs:   <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>

SELECT ?feed ?name (COUNT(?element) AS ?elements)
WHERE {
    ?feed a schema:DataFeed ;
        schema:dataFeedElement ?element .
    OPTIONAL { ?feed schema:name ?name }
}
GROUP BY ?feed ?name
ORDER BY DESC(?elements)
```

### Exclude the tiny ones

```shmarql
# shmarql-view: piechart
# shmarql-names: name
# shmarql-values: elements
# shmarql-label: Instance Count

PREFIX rdfs:   <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>

SELECT ?feed ?name (COUNT(?element) AS ?elements)
WHERE {
    ?feed a schema:DataFeed ;
        schema:dataFeedElement ?element .
    OPTIONAL { ?feed schema:name ?name }

    # Grand total across every feed's elements, cross-joined into each group.
    {
    SELECT (COUNT(*) AS ?total) WHERE {
        ?f a schema:DataFeed ; schema:dataFeedElement ?e .
    }
    }
}
GROUP BY ?feed ?name ?total
HAVING (COUNT(?element) * 100 >= ?total)
ORDER BY DESC(?elements)
```

### Only show the tiny ones

```shmarql
# shmarql-view: piechart
# shmarql-names: name
# shmarql-values: elements
# shmarql-label: Instance Count

PREFIX rdfs:   <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>

SELECT ?feed ?name (COUNT(?element) AS ?elements)
WHERE {
    ?feed a schema:DataFeed ;
        schema:dataFeedElement ?element .
    OPTIONAL { ?feed schema:name ?name }

    # Grand total across every feed's elements, cross-joined into each group.
    {
    SELECT (COUNT(*) AS ?total) WHERE {
        ?f a schema:DataFeed ; schema:dataFeedElement ?e .
    }
    }
}
GROUP BY ?feed ?name ?total
HAVING (COUNT(?element) * 100 <= ?total)
ORDER BY DESC(?elements)
```
