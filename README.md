# Who's got dirt?

This project aims to define a common way of querying a variety of data providers about information regarding people and companies. The goal is to enable easy lookup of entities across multiple databases, and to support the integration and enrichment of entity-related data.

## Existing efforts

* [Popolo](http://www.popoloproject.com/) data standard for people and organizations.
* Linked Data, [Linked Data Fragments](http://linkeddatafragments.org/), FOAF, [SPARQL](http://www.w3.org/TR/rdf-sparql-query/)
* OpenRefine [Reconciliation API](https://github.com/OpenRefine/OpenRefine/wiki/Reconciliation-Service-API) for fuzzy entity matching.
* [Metaweb Query Language](http://wiki.freebase.com/wiki/MQL), a more sophisticated query mechanism which supports matching entities in terms of their connections to other nodes in a graph. Also: [CYPHER](http://neo4j.com/developer/cypher-query-language/).
* Survey of [partner project data models](https://docs.google.com/spreadsheets/d/1on99aF9QVWOwqZDtla9RLX5Wza6MHxbb4apZwbtHK-w/edit).
* [IATI OrgID](http://iatistandard.org/201/codelists/OrganisationRegistrationAgency/) scheme for company identification.
* [OpenCorporates API](https://api.opencorporates.com/), [LittleSis API](http://api.littlesis.org/).

## Discussion pieces

* [A Call for Collaboration: Data Mining in Cross-Border Investigations](http://gijn.org/2015/05/19/a-call-for-collaboration-data-mining-in-cross-border-investigations/)
* [Who’s got dirt: What if robots could do cross-border investigations?](https://ijnet.org/en/blog/who%E2%80%99s-got-dirt-what-if-robots-could-do-cross-border-investigations)

## Use cases

* Use cases for the [Influence Mapping Data Specification](https://docs.google.com/document/d/1PBH9WvwiJ899hJHCxoEdg6Ty6HHF21xYxi5QGgv5rak/edit#).
* Use cases for the [trade negotiations case study](https://docs.google.com/document/d/11FMG2KlNigZkMhmdlHo1KynNAP8cY4hYEJCtYXBPsCI/edit).

## Proposed modules

Some simple conventions for the API could be: 

* Use HTTP and JSON, define a single endpoint and use HATEOAS from there.
* Re-use as much spec as possible (Popolo, Freebase, ...)
* Define different levels of compliance (e.g. so that databases that do not support graph queries can express their capabilities). 
* Speak of consumers and producers, rather than client/server.

### Query API

* Asking for information about an entity
* Asking for entities in terms of their connections

* Proposal: re-use MQL instead of the reconciliation API, because it supports graph queries.

### Response format

Given the diverse nature of data providers and use cases, we probably need to support different response levels:

* Full data: the API will return a full, structured data record or set of records to match the query.
* Document/reference list - data maintained by the service may not be fully structured, so the returned information would be limited to document references which can be accessed by a researcher.
* Information holder contact, in scenarios where the result data itself cannot be shared, contact information for the person holding the relevant material should be indicated.

* Proposal: separate "record" and "result envelope"
    * Result envelope holds scoring, provenance, licensing info
    * (Optional) record would be Popolo or DublinCore?

### Data model extensions

While Popolo provides a data model for ``Persons``, ``Organizations`` and ``Memberships``, it is mainly structured around parliamentary information use cases.

It does not currently have a notion of ``Companies``, nor does it define relevant connection types such as ``Control`` (e.g. for ownership) and ``Transaction`` (e.g. for contracts). For a more detailed analysis of potential connection types, see [James' inventory of terms](https://docs.google.com/spreadsheets/d/1on99aF9QVWOwqZDtla9RLX5Wza6MHxbb4apZwbtHK-w/edit#gid=917587019).  

## Getting it adopted

* Build a set of proxies around existing data providers (i.e. partners, but also ICIJ Offshore, CorpWatch CrocTail - even WhoIs)
* Prototype client applications around different patterns of use
    * Search: simple lookup, return all matchin results
    * Enrich: add edges to a graph in a visual utility
    * Lead list: generate a set of graph snippets matching a particular pattern
    * Notification: a scheduled job to track changes in service responses

### Who should be involved?

* Partners of Influence Mappers (OpenCorporates, Poderopedia, LittleSis, OpenNorth, American Academy)
* International Consortium of Investigative Journalists and the Organized Crime and Corruption Reporting Project 
* Consumer-side apps: detective.io, kumu.io, linkurio.us
* Producer-side apps: Overview, DocumentCloud, PopIt, Investigative Dashboard, CrocTail
