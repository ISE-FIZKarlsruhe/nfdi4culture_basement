# NFDI4Culture Ontology (CTO)

**Version:** 3.0.0  
**Namespace:** `https://nfdi4culture.de/ontology#`  
**Prefix:** `cto:`  
**License:** CC0 1.0 (Public Domain)  
**Created:** 2025-03-31  
**Documentation:** https://nfdi.fiz-karlsruhe.de/4culture/

**Authors:** Harald Sack, Tabea Tietz, Oleksandra Bruns, Jörg Waitelonis, Etienne Posthumus  
**Contributors:** Torsten Schrade, Linnaea Söhn, Jonatan Jalle Steller

---

## Overview

The NFDI4Culture Ontology (CTO) models cultural heritage research resources with the goal of enabling a **research data index** for the NFDI4Culture consortium — the German National Research Data Infrastructure for culture. It describes and interlinks the diverse entities encountered in cultural heritage research (meta)data, including persons, organizations, places, events, media objects, legal statements, external vocabulary references, and musical incipits.

CTO is a **domain-specific module** built on two upper layers:

- **NFDIcore v3** (`https://nfdi.fiz-karlsruhe.de/ontology/`) — the mid-level ontology shared across NFDI consortia, providing general classes for agents, resources, services, projects, and roles.
- **Basic Formal Ontology 2020 (BFO)** (`http://purl.obolibrary.org/obo/`) — the upper-level ontology providing the foundational continuant/occurrent distinction.

The three-level stack is: **BFO → NFDIcore → CTO**

---

## Ontological Foundations

### BFO (Basic Formal Ontology)

CTO inherits BFO's core distinction between:

- **Continuants** — entities that persist through time (material entities, roles, qualities, generically dependent continuants such as information content entities).
- **Occurrents** — entities that unfold in time (processes, events, temporal regions).

Key BFO classes used in CTO include:

| BFO Class                            | Description                                                                        |
| ------------------------------------ | ---------------------------------------------------------------------------------- |
| `bfo:IndependentContinuant`          | Entities existing without depending on another (material objects, spatial regions) |
| `bfo:GenericallyDependentContinuant` | Information content entities, plans, specifications                                |
| `bfo:Role`                           | Relational properties that inhere in agents contextually                           |
| `bfo:Process`                        | Temporally extended events and activities                                          |
| `bfo:SpatialRegion`                  | Places and geographic regions                                                      |

Key BFO object properties used throughout the ontology:

| Property                                 | Description                                        |
| ---------------------------------------- | -------------------------------------------------- |
| `bfo:has part of` / `continuant part of` | Mereological parthood                              |
| `bfo:inheres in` / `bearer of`           | Quality/role inhering in an independent continuant |
| `bfo:has role` (`BFO_0000196`)           | Relates an entity to a role it bears               |
| `bfo:realizes` (`BFO_0000054`)           | Links a process to the role it realizes            |
| `bfo:is about` (`IAO_0000136`)           | Information content entity aboutness               |
| `bfo:generically depends on`             | Dependence of information content on a bearer      |

---

## Module: NFDIcore

NFDIcore provides mid-level infrastructure classes reused across NFDI consortia. CTO specializes many of these.

### Core NFDIcore Classes

#### Agents and Persons

| Class           | URI suffix     | Description                                                                                         |
| --------------- | -------------- | --------------------------------------------------------------------------------------------------- |
| `Agent`         | `NFDI_0000102` | Independent continuant capable of autonomous action; equivalent to any entity bearing an agent role |
| `Person`        | `NFDI_0000004` | A human individual                                                                                  |
| `Organization`  | `NFDI_0000003` | A formally constituted group or institution                                                         |
| `Contributor`   | `NFDI_0000231` | An agent that contributes to a project or work                                                      |
| `Contact point` | `NFDI_0001005` | An agent acting as communication interface                                                          |

#### Places

| Class     | URI suffix     | Description                      |
| --------- | -------------- | -------------------------------- |
| `Place`   | `NFDI_0000005` | A geographic or spatial location |
| `Country` | `NFDI_0000119` | A sovereign territory            |
| `City`    | `NFDI_0000106` | A densely populated urban area   |

#### Resources and Documents

| Class                    | URI suffix     | Description                                                                                 |
| ------------------------ | -------------- | ------------------------------------------------------------------------------------------- |
| `Resource`               | `NFDI_0000001` | Top-level information entity                                                                |
| `Document`               | `NFDI_0000022` | A formally structured written or digital work                                               |
| `Data portal`            | `NFDI_0000123` | An online platform centralizing access to datasets (e.g. Bach digital, KultSam, MusiXplora) |
| `File data item`         | `NFDI_0000027` | A data item representing a file stored on disk                                              |
| `Report`                 | `NFDI_0000029` | A structured document communicating findings                                                |
| `Source code repository` | `NFDI_0000030` | A repository managing software and version history                                          |
| `Project proposal`       | `NFDI_0001004` | A document describing objectives and scope of a project                                     |

#### Events

| Class   | URI suffix     | Description                                |
| ------- | -------------- | ------------------------------------------ |
| `Event` | `NFDI_0000131` | An occurrence at a point or period in time |

#### Software and Technology

| Class               | URI suffix     | Description                                               |
| ------------------- | -------------- | --------------------------------------------------------- |
| `Software`          | `NFDI_0000198` | A software application                                    |
| `Database software` | `NFDI_0000121` | Software providing access to structured data repositories |
| `Software title`    | `NFDI_0001000` | A textual identifier for a software application           |
| `Technology`        | `NFDI_0000215` | A technology used by a resource                           |

#### Services

| Class                   | URI suffix     | Description                                                      |
| ----------------------- | -------------- | ---------------------------------------------------------------- |
| `Service product`       | `NFDI_0000232` | An immaterial entity delivering value through a service offering |
| `Service process`       | `NFDI_0000101` | A process organizing activities to deliver a service             |
| `Service specification` | `NFDI_0000244` | A plan specifying requirements of a service                      |
| `Data curation service` | `NFDI_0000122` | A service managing and promoting data use over its lifecycle     |

#### Identifiers and Authority Files

| Class             | URI suffix     | Description                                     |
| ----------------- | -------------- | ----------------------------------------------- |
| `GND identifier`  | `NFDI_0001009` | German Integrated Authority File identifier     |
| `VIAF identifier` | `NFDI_0001010` | Virtual International Authority File identifier |

#### Academic Disciplines

| Class                 | URI suffix     | Description                                               |
| --------------------- | -------------- | --------------------------------------------------------- |
| `Academic discipline` | `NFDI_0000100` | An information content entity specifying a subject domain |
| `Humanity`            | `NFDI_0010001` | Disciplines covering literature, philosophy, arts         |
| `Social science`      | `NFDI_0010002` | Disciplines covering society and human behaviour          |
| `Natural science`     | `NFDI_0010007` | Disciplines studying the natural world                    |

#### Roles

| Class                        | URI suffix           | Description                                           |
| ---------------------------- | -------------------- | ----------------------------------------------------- |
| `Agent role`                 | `NFDI_0000229`       | A role realized through an agent's actions            |
| `Creator role`               | (via `NFDI_0001026`) | Role realized when creating something                 |
| `Contributor role`           | `NFDI_0000118`       | Role realized when contributing to a work or project  |
| `Contact point role`         | `NFDI_0000114`       | Role as a communication interface                     |
| `Service provider role`      | `NFDI_0000230`       | Role realized when delivering services                |
| `Consortium member role`     | `NFDI_0000110`       | Role of an organization as consortium member          |
| `Organizer role`             | `NFDI_0001061`       | Role realized when organizing an event or project     |
| `Co-organizer role`          | `NFDI_0001057`       | Shared organizing role                                |
| `Speaker role`               | `NFDI_0001062`       | Role realized when delivering presentations at events |
| `Co-spokesperson role`       | `NFDI_0001058`       | Shared public representation role                     |
| `Collaboration partner role` | `NFDI_0001059`       | Role in cooperative work toward shared goals          |
| `Manufacturer role`          | `NFDI_0001060`       | Role of producing goods or components                 |

#### Specification Classes

| Class                                | URI suffix     | Description                                                  |
| ------------------------------------ | -------------- | ------------------------------------------------------------ |
| `Event implementation specification` | `NFDI_0001050` | Guidelines for executing events (virtual, hybrid, in-person) |
| `Funding specification`              | `NFDI_0001051` | Policies for managing financial resources for activities     |
| `Information access specification`   | `NFDI_0001052` | Policies governing access to information resources           |
| `Metadata specification`             | `NFDI_0001054` | Guidelines for metadata structure and usage                  |
| `Status specification`               | `NFDI_0001056` | Defines current state of an entity or project                |
| `Workflow specification`             | `NFDI_0001002` | Defines task sequence and dependencies in a workflow         |

#### Named Individuals (Specification Instances)

The ontology includes enumerated instances for several specification types:

**Access types** (instances of `Information access specification`): Open access, Restricted access, Paywall access.

**Funding types** (instances of `Funding specification`): Project funding, Self funding.

**Status values** (instances of `Status specification`): Planning status, Test phase status, and others.

**Academic titles** (instances of `NFDI_0001040`): Dr., Prof., Prof. Dr., Dr. habil., PD. Dr., Dr. Dr., and variants — for use with persons.

---

## Module: CTO (NFDI4Culture-specific)

The CTO namespace (`https://nfdi4culture.de/ontology/`) introduces classes and properties specific to cultural heritage research data indexing.

### Core CTO Classes

#### Source Item and Data Feed Model

| Class         | URI           | Description                                                                                                                                               |
| ------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Source item` | `CTO_0001005` | The central class representing any cultural heritage item described by a data provider. Source items are the primary subjects of the research data index. |
| `Lyrics`      | `CTO_0001002` | A textual entity representing the lyrics associated with a musical work                                                                                   |
| `Incipit`     | `CTO_0001024` | A musical incipit — the opening passage of a work, encoded in musical notation for identification in catalogs and databases                               |

The data feed integration model reuses Schema.org:

- `schema:DataFeed` — a feed of data items from a provider
- `schema:DataFeedItem` — a wrapper around each item in the feed
- `schema:item` — links a `DataFeedItem` to a `CTO_0001005` (Source item)
- `schema:dataFeedElement` — links a `DataFeed` to its `DataFeedItem` elements

#### External Classification

| Class                 | URI           | Description                                                                                                                 |
| --------------------- | ------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `External classifier` | `CTO_0001027` | A classification concept from an external vocabulary system (e.g. Getty AAT, RISM, Iconclass) used to classify source items |

### CTO Object Properties

#### Source Item Relations

| Property                                          | Domain        | Range                            | Description                                                                                                                                       |
| ------------------------------------------------- | ------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `has related entity`                              | `CTO_0001005` | any                              | General relation from a source item to any referenced entity (existence-only, role-agnostic)                                                      |
| `has related person`                              | `CTO_0001005` | `Person`                         | Source item references a person (composer, spouse, supervisor, etc.)                                                                              |
| `has related organization`                        | `CTO_0001005` | `Organization`                   | Source item references an organization (publisher, ensemble, university, etc.)                                                                    |
| `has related location`                            | `CTO_0001005` | `Place`                          | Source item references a geographic location                                                                                                      |
| `has related event`                               | `CTO_0001005` | `Event`                          | Source item references an event (concert, premiere, festival, etc.)                                                                               |
| `has related item`                                | `CTO_0001005` | `ICE`                            | Source item references another item (co-signed documents, related manuscripts, etc.)                                                              |
| `is related entity of`                            | any           | `CTO_0001005`                    | Inverse of `has related entity`                                                                                                                   |
| `has holding organization`                        | `CTO_0001005` | `Organization`                   | Links an item to the institution (library, archive, museum) holding it                                                                            |
| `is referenced in`                                | `CTO_0001005` | `DataFeed`                       | Source item is described in the given data feed                                                                                                   |
| `has associated media` (`schema:associatedMedia`) | `CTO_0001005` | `MediaObject`                    | Links a source item to image or audio media objects                                                                                               |
| `is associated media of`                          | `MediaObject` | `CTO_0001005`                    | Inverse of `associatedMedia`                                                                                                                      |
| `is about real world entity`                      | `ICE`         | Person/Event/Book/Sculpture/etc. | A source item URI is about a specific real-world entity                                                                                           |
| `has data concept`                                | `ICE`         | `skos:Concept`                   | Links a source item to an overarching SKOS concept category used in the NFDI4Culture Data Search (e.g. "3D model", "sheet music", "audio object") |

#### Classification

| Property                  | Description                                                                                                                          |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `has external classifier` | Relates a thing to its entry in an external classification scheme (Getty, RISM, Iconclass, etc.); subproperty of `IAO:is_denoted_by` |
| `is classified by`        | Inverse direction: source item is classified by an external classification system                                                    |

#### Musical Incipit

| Property        | Description                                         |
| --------------- | --------------------------------------------------- |
| `has incipit`   | Relates a source item to its musical incipit entity |
| `is incipit of` | Inverse: an incipit belongs to a source item        |

#### Lyrics

| Property       | Description                              |
| -------------- | ---------------------------------------- |
| `has lyrics`   | Relates a source item to a Lyrics entity |
| `is lyrics of` | Inverse                                  |

#### Data Feed Structure

| Property                  | Description                                                 |
| ------------------------- | ----------------------------------------------------------- |
| `is data feed element of` | A data feed element is a continuant part of a DataFeed      |
| `is item of`              | Determines the subject of an ICE that is part of a DataFeed |

### CTO Data Properties

#### Legal and Rights

| Property                | Description                                                          |
| ----------------------- | -------------------------------------------------------------------- |
| `has legal statement`   | General superproperty for legal and rights-related statements (text) |
| `has license statement` | Natural language description of license terms                        |
| `has rights statement`  | Textual or URI-based copyright/IP statement                          |

#### Media

| Property          | Description                                                                           |
| ----------------- | ------------------------------------------------------------------------------------- |
| `has content url` | URL location of a media object associated with a source item (e.g. an image file URL) |

#### Temporal

| Property                 | Description                                                                                   |
| ------------------------ | --------------------------------------------------------------------------------------------- |
| `has temporal coverage`  | Generic temporal attribute (period of existence, approximate dates); use for vague references |
| `has approximate period` | Estimated or imprecise date when exact date is unknown                                        |
| `has creation date`      | Precise `xsd:dateTime` when an item came into existence                                       |

#### Archival

| Property         | Description                                                       |
| ---------------- | ----------------------------------------------------------------- |
| `has shelf mark` | Alphanumeric shelf location code assigned by a library or archive |

#### Musical Incipit Data Properties

These all have domain `CTO_0001024` (Incipit):

| Property                     | Description                                                                              | Example                         |
| ---------------------------- | ---------------------------------------------------------------------------------------- | ------------------------------- |
| `has incipit value`          | The full literal representation of the incipit (key, pattern, clef, time, key signature) |                                 |
| `has incipit pattern`        | Melodic/rhythmic pattern string                                                          | `"4'D/8.6GB4AG/8.6''EC2'A/..."` |
| `has incipit time signature` | Time signature string                                                                    | `"3/4"`                         |
| `has clef`                   | Clef at the start of the incipit                                                         | `"G-2"`                         |
| `has key signature`          | Written accidentals at the start                                                         | `"xF"`                          |
| `has key`                    | Tonal key of the passage                                                                 | `"G major"`                     |

#### Lyrics Data Properties

| Property          | Description                        |
| ----------------- | ---------------------------------- |
| `has lyrics text` | The text string of a Lyrics entity |

### SKOS Concepts (Named Individuals — Data Types)

CTO defines a small controlled vocabulary of SKOS `Concept` instances representing broad data types used by the NFDI4Culture Data Search facets:

| Concept      | Description |
| ------------ | ----------- |
| 3D model     |             |
| Audio object |             |
| Image object |             |
| Media object |             |
| Sheet music  |             |
| Text object  |             |
| Video object |             |

These are attached to source items via the `has data concept` property and support faceted search across the research data index.

---

## NFDIcore Object Properties (Selection)

Key properties inherited from NFDIcore that CTO uses extensively:

| Property                      | Description                                                                           |
| ----------------------------- | ------------------------------------------------------------------------------------- |
| `has external identifier`     | Relates a thing to an entry in an authority register (GND, VIAF, RISM, Wikidata, ROR) |
| `is identified by`            | Inverse of `has external identifier`                                                  |
| `has creator`                 | Links a created thing to its creating agent                                           |
| `creator of`                  | Inverse                                                                               |
| `has agent`                   | Relates a resource to an agent                                                        |
| `is agent of`                 | Inverse                                                                               |
| `has subject area`            | Relates a resource to its academic discipline or domain                               |
| `has standard`                | Relates a resource to a standard it relies on                                         |
| `has specification`           | Relates a resource to an international standard or norm                               |
| `has technology`              | Relates a resource to a technology it uses                                            |
| `has software`                | Relates a resource to associated software                                             |
| `has sparql endpoint`         | Relates a data resource to its SPARQL endpoint                                        |
| `has subsidiary organization` | Represents subsidiary/ownership relationships between organizations                   |
| `is subject of`               | Inverse of `is about` — the top-level "reverse aboutness" property                    |

### NFDIcore Data Properties

| Property          | Domain         | Description                                          |
| ----------------- | -------------- | ---------------------------------------------------- |
| `birth date`      | `Person`       | Date of birth (literal, accommodates imprecise data) |
| `death date`      | `Person`       | Date of death (literal)                              |
| `foundation date` | `Organization` | Date of official establishment (literal)             |
| `has value`       | `ICE`          | The specific literal value of an information entity  |
| `has url`         | `ICE`          | A URL (`xsd:anyURI`) associated with an entity       |
| `file extension`  | `Resource`     | File format suffix (e.g. `ttl`, `xml`, `mp4`)        |

---

## Alignment with External Vocabularies

CTO explicitly imports and reuses terms from:

- **Schema.org** — `DataFeed`, `DataFeedItem`, `MediaObject`, `Book`, `Sculpture`, `SheetMusic`, `TheaterEvent`, `LandmarksOrHistoricalBuildings`, `associatedMedia`, `dataFeedElement`, `item`, `license`
- **SKOS** — for concept-based classification
- **IAO** (Information Artifact Ontology) — for information content entities, identifiers, plans, directives
- **EDAM** — format classes (`JSON-LD`, `N-Quads`, `Turtle`) for describing data formats
- **Dublin Core Terms** — bibliographic metadata on the ontology itself

The `has external classifier` / `is classified by` property pair is specifically designed to support controlled vocabulary systems such as **Iconclass**, **Getty AAT**, **RISM**, and similar classification schemes used in cultural heritage.

---

## Key Modelling Patterns

### Research Data Index Pattern

The central pattern is the triple: **DataFeed → DataFeedItem → SourceItem**.

```
schema:DataFeed
  schema:dataFeedElement  →  schema:DataFeedItem
                                  schema:item  →  cto:SourceItem
                                                     cto:has related person  →  nfdi:Person
                                                     cto:has related event   →  nfdi:Event
                                                     cto:has related location →  nfdi:Place
                                                     cto:has data concept    →  skos:Concept
                                                     schema:associatedMedia  →  schema:MediaObject
                                                     cto:has external classifier  →  cto:ExternalClassifier
```

### Role Pattern (BFO-style)

Agents bear roles that are realized in processes. This enables flexible modelling of activities without commitment to specific properties on agents:

```
nfdi:Person
  bfo:has role  →  nfdi:CreatorRole
                      bfo:realized in  →  bfo:Process (creation activity)
```

### Musical Incipit Pattern

```
cto:SourceItem
  cto:has incipit  →  cto:Incipit
                          cto:has incipit pattern    "4'D/8.6GB..."
                          cto:has clef               "G-2"
                          cto:has key                "G major"
                          cto:has key signature      "xF"
                          cto:has incipit time signature "4/4"
```

---

## Namespace Prefixes

| Prefix    | Namespace                                     |
| --------- | --------------------------------------------- |
| `cto:`    | `https://nfdi4culture.de/ontology/`           |
| `nfdi:`   | `https://nfdi.fiz-karlsruhe.de/ontology/`     |
| `bfo:`    | `http://purl.obolibrary.org/obo/`             |
| `iao:`    | `http://purl.obolibrary.org/obo/` (IAO terms) |
| `schema:` | `https://schema.org/`                         |
| `skos:`   | `http://www.w3.org/2004/02/skos/core#`        |
| `dct:`    | `http://purl.org/dc/terms/`                   |

---

## Versioning

| Field           | Value                                    |
| --------------- | ---------------------------------------- |
| Current version | 3.0.0                                    |
| Prior version   | 2.2.0                                    |
| Version IRI     | `https://nfdi4culture.de/ontology/3.0.0` |
| OWL API         | 5.1.18                                   |

Note, the above documenation was generated from the .ttl file of the ontology with the following prompt by Anthropic Claude Sonnet 4.6 Low:

```
Please make a documentation file in markdown of this ontology, explaining it's key features and listing the various properties and classes and how they relate
```
