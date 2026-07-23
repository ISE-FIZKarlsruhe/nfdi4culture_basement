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
