# TODO

Here is a general to-do list for the tasks that need to be done in this documentation repository.
Meant as a reminder, not an exhaustive list. Ordering does not currently imply priority.

- [ ] Find the Git repo location for each datafeed, and list them.

- [ ] Explain how the IIIF and image handling is done.

- [ ] Work out an entity resolution pipeline, and document it.

- [ ] Make a demo of how it is possible to use OTTR templates via maplib for triple generation

## Ideas to follow up on

Can we add a list of prefixes for actual data feeds so that when we are viewing and displaying them in downstream applications like browsing the knowledge graph to have more human-friendly short prefixes?
For example, Detmolder Hoftheater: https://portal.hoftheater-detmold.de/index.html#HoftheaterDetmold:werk_H020974.xml becomes `E5305:werk_H020974.xml` etc.
But then this has to be maintained somewhere over ingestions in the Kitchen pipeline

## Data modelling

- Stop using blank nodes for things like 'Person'. If there is a blank node, it is not possible to "point" to a specific item in the KG, and you can not query for it, without referencing "where it comes from" So, we need to hurry up with the entity disambiguation and mint throw-away URIs that can be merged. (do we then save tomb-stones for the deleted items with a same-as ?)
