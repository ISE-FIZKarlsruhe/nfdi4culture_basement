# Searching in the Knowledge Graph

Within SHMARQL we are using [FIZzySearch](https://github.com/ISE-FIZKarlsruhe/fizzysearch) and [bikiDATA](https://github.com/ISE-FIZKarlsruhe/bikidata) to overlay advanced SPARQL query features, not available in the underlying triple stores, into the searches.

It adds these extra search modalities by rewriting the queries before sending them off to the triplestore for processing.

## Full Text (BM25)

## Text embeddings

## Image embeddings

Using CLIP, also see [other page](images.md) on this topic.

## Triple embeddings

Similar to [RDF2Vec](https://rdf2vec.org/), but tuned for larger datasets.
We are working on an implementation that can handle large graph sizes efficiently. Current code [here](https://github.com/ISE-FIZKarlsruhe/bikidata/blob/fix_embeds/src/bikidata/rdf2vec.py).

Looking at the situation in July 2026, we had 107_849_417 triples over 18_942_001 entities. Generating 100 random walks of length 15 on teach2 in paralllel using all cores and an efficient C-based [graph library](https://igraph.org/) still takes circa 800 seconds per batch of 1000 items. That would mean (18_942_001 \* 800 / 1000 / 60 / 60 / 24) = ± 175 days to just generate the random walks. We can't wait that long. How can we speed this up more? Where is the bottleneck?

Do we really need to have **all** the triples in memory when we are doing the random walks, or can we get away with "trimming" away certain properties? For example, if we exclude the schema:dataFeedElements, it goes down to 95_488_329 / 107_849_417, already a reduction to 88%
But we need a bigger improvement, this is still not enough. We will ask Claude for some tuning advice...

The biggest improvement came after asking Claude to rewrite the random walk code in [Go](https://go.dev/) instead of Python. In a simple test on my laptop, the Python code was doing around 0.7 seconds per entity, and the Go version is 0.0003 seconds per entity. A huge win.

### Issues with the random walks

After running a probe for the generated walks, with this query:

```sql
SELECT I.value, t2.W, t1.pos_idx, t2.pos_idx
           FROM random_walks rw,
               unnest(rw.walks) with ordinality AS t1(walk, pos_idx),
               unnest(t1.walk) with ordinality AS t2(W, pos_idx)
           JOIN iris I ON I.hash = t2.W
           WHERE rw.s = 1034366998712611147 and t1.pos_idx = 99 order by t1.pos_idx, t2.pos_idx;
```

we get:

| IRI                                                           | W                    | Walk ID | Position |
| ------------------------------------------------------------- | -------------------- | ------- | -------- |
| `<https://foto.biblhertz.it/media/obj/08103687/bh017983>`     | 1034366998712611147  | 99      | 1        |
| `<https://nfdi4culture.de/id/ark:/60538/E6064_c1462814>`      | 12566023639421539838 | 99      | 2        |
| `<https://nfdi4culture.de/id/E6064>`                          | 16282542170408223152 | 99      | 3        |
| `<https://foto.biblhertz.it/media/obj/08003521/bhim00027041>` | 15132840929786169099 | 99      | 4        |
| `<https://nfdi4culture.de/id/ark:/60538/E6064_5f3880ef>`      | 1049492907556676324  | 99      | 5        |
| `<https://nfdi4culture.de/id/E6064>`                          | 16282542170408223152 | 99      | 6        |
| `<https://foto.biblhertz.it/media/obj/08065106/bh422073>`     | 1571090462958131627  | 99      | 7        |
| `<http://schema.org/ImageObject>`                             | 822532815216855555   | 99      | 8        |
| `_:f9e9c9eda83b5b3d91f5792d58b1a95e`                          | 1107542925987357567  | 99      | 9        |
| `_:f9e9c9eda83b5b3d91f5792d58b1a95e`                          | 1107542925987357567  | 99      | 11       |
| `<https://foto.biblhertz.it/media/obj/08037941/bhpd27710>`    | 13476784914288269516 | 99      | 12       |
| `<https://nfdi4culture.de/id/E6367>`                          | 7316804296421606056  | 99      | 13       |
| `<https://lod.academy/epidat/id/hha-4611>`                    | 4081495844673460313  | 99      | 14       |
| `<https://lod.academy/epidat/id/hha-4611>`                    | 4081495844673460313  | 99      | 16       |

But if we strictly look at the triples in `<https://foto.biblhertz.it/media/obj/08103687/bh017983>` it does not have reference to `<https://nfdi4culture.de/id/ark:/60538/E6064_c1462814>` - but it does vice-versa. So the random walk sees the graph as undirected, and generates walks accordingly. But this is not how triples necessary work. How problematic is this? Should we try and fix it?

Also, consider the giant entities like `<https://nfdi4culture.de/id/E6064>` - it has 349136 associated triples!
This is a known modelling problem which needs it's own discussion page.

### Performance issues training the word2vec on the NFDI4Culture-size KG

It is just too slow, and single-threaded. So we need to investigate GPU accelerated alternatives.

### Data Selection

As mentioned previously when considering excluding the dataFeedElement items, there might be significant improvements if we only calc embeddings for certain items. After all, we only want to get "more like this" kind of suggestions for certain things like people or places. Is this a correct assumption? Let's try and see what kinds of things there actually are in the KG.

#### People?

As a start, how many nfdicore:NFDI_0000004 (People) are there?

```shmarql
SELECT count(distinct(?person)) as ?cont
  WHERE {
  ?person ?p <https://nfdi.fiz-karlsruhe.de/ontology/NFDI_0000004> .
}
```

There are many persons with blanknodes as identifiers.

```shmarql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>

SELECT DISTINCT ?linked ?label ?pp ?person ?o
WHERE {
  ?person ?linkProp <https://nfdi.fiz-karlsruhe.de/ontology/NFDI_0000004> .
  ?person rdfs:label ?o .
  ?linked ?pp ?person .
  OPTIONAL {
    ?linked rdfs:label ?label .
  }
  FILTER (isBlank(?person))
}
LIMIT 32
OFFSET 2000
```
