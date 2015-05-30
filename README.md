# Who's got dirt?

This project aims to define a common way of querying a variety of data sources about information regarding people and companies. The goal is to enable easy lookup of entities across multiple databases, and to support the integration and enrichment of entity-related data.

## Existing efforts

* [Popolo](http://www.popoloproject.com/) data standard for people and organizations.
* [IATI OrgID](http://iatistandard.org/201/codelists/OrganisationRegistrationAgency/) scheme for company identification.
* OpenRefine [Reconciliation API](https://github.com/OpenRefine/OpenRefine/wiki/Reconciliation-Service-API) for fuzzy entity matching.
* [Metaweb Query Language](http://wiki.freebase.com/wiki/MQL), a more sophisticated query mechanism which supports matching entities in terms of their connections to other nodes in a graph.
* Survey of [partner project data models](https://docs.google.com/spreadsheets/d/1on99aF9QVWOwqZDtla9RLX5Wza6MHxbb4apZwbtHK-w/edit).

## Discussion pieces

* [A Call for Collaboration: Data Mining in Cross-Border Investigations](http://gijn.org/2015/05/19/a-call-for-collaboration-data-mining-in-cross-border-investigations/)
* [Who’s got dirt: What if robots could do cross-border investigations?](https://ijnet.org/en/blog/who%E2%80%99s-got-dirt-what-if-robots-could-do-cross-border-investigations)

## Use cases

* Use cases for the [Influence Mapping Data Specification](https://docs.google.com/document/d/1PBH9WvwiJ899hJHCxoEdg6Ty6HHF21xYxi5QGgv5rak/edit#).
* Use cases for the [trade negotiations case study](https://docs.google.com/document/d/11FMG2KlNigZkMhmdlHo1KynNAP8cY4hYEJCtYXBPsCI/edit).

## Key problems / modules

### Query API

* Asking for information about an entity
* Asking for entities in terms of their connections

### Response format

* Different response levels
    * Full data: the API will return a full, structured data record or set of records to match the query.
    * Document/reference list - data maintained by the service may not be fully structured, so the returned information would be limited to document references which can be accessed by a researcher.
    * Information holder contact, in scenarios where the result data itself cannot be shared, contact information for the person holding the relevant material should be indicated.

* Proposal: separate "record" and "result envelope"
    * Result envelope holds scoring, provenance, licensing info
    * (Optional) record would be Popolo

## Getting it adopted


### Who should be involved?

* Partners of Influence Mappers (OpenCorporates, Poderopedia, LittleSis, OpenNorth, American Academy)
* International Consortium of Investigative Journalists and the Organized Crime and Corruption Reporting Project (Investigative Dashboard)
* Client-side apps: detective.io, kumu.io, linkurio.us
* Core apps: Overview
