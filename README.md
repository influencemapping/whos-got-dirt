# Who's got dirt?

Many (open) databases publish information about companies, institutions and people to the web. This might include details about company ownership, asset ownership, political finance, government contracts, and many other sources of information about political and economic influence.

Yet our story about how researchers would access these databases is incomplete: would a journalist visit all possible data sites each time they want to look up a set of people or companies? We want to automate this lookup by making sure many of our sites support one simple API call for asking: who knows something about this entity, who's got dirt?

This repository defines a common way of querying data providers about people and companies. The goal is to make lookups of entities across multiple databases easy, and to support consumer tools when they have to merge the data they find into the data they already have.

## What is it for?

The standard will allow consumer tools to automatically search and integrate information about companies and people from data providers. This can be useful in the context of various activities:

* Searching: generate a simple list of candidates containing all matching records from a variety of sources.
* Enrichment: rather than just generating a results list, integrate all relevant data availabale in a data provider system into the consumer tool's data model.
* Lead list: rather than just querying for entities by name, find entities and relationships that match certain graph patterns.
* Notification: a scheduled job to track changes in service responses, i.e. new entities or relationships.

### Detailed use cases

There are also more detailed documents to detail use cases for the [Influence Mapping Data Specification](https://docs.google.com/document/d/1PBH9WvwiJ899hJHCxoEdg6Ty6HHF21xYxi5QGgv5rak/edit#) and for the [trade negotiations case study](https://docs.google.com/document/d/11FMG2KlNigZkMhmdlHo1KynNAP8cY4hYEJCtYXBPsCI/edit).

### Discussion

* [A Call for Collaboration: Data Mining in Cross-Border Investigations](http://gijn.org/2015/05/19/a-call-for-collaboration-data-mining-in-cross-border-investigations/)
* [Who’s got dirt: What if robots could do cross-border investigations?](https://ijnet.org/en/blog/who%E2%80%99s-got-dirt-what-if-robots-could-do-cross-border-investigations)

## How does it work?

Data providers will implement a single API endpoint, with different degrees of completeness (based on the type of data they store, and the capabilities of their tools). The basic mechanism uses query-by-example, i.e. the user will submit all the known properties (e.g. the name) of an entity and expect a list of suggested matches as a response.

Since a common data schema is required for this mechanism, all data will be transferred using the JSON representation of [Popolo](http://www.popoloproject.com/), a standard way of expressing data about people, organisations and the relationships between them. The Influence Mappers team will extend the field and class definitions of Popolo over the course of the project.

### A simple query

Let's look at a query for five  matches amongst ``Organizations`` sorted by the edit distance between the name ``Big Corp`` and the entity name:

```json
{
    "queries": {
        "q0": {
            "name~=": "Big Corp",
            "type": "popolo:Organization",
            "limit": 5
        }
    }
}
```

The syntax for this query is the [Metaweb Query Language](http://wiki.freebase.com/wiki/MQL) used by Freebase to formulate graph queries against it's knowledge base. It is thus a more powerful variant of the better-known [Refine Reconciliation API](https://github.com/OpenRefine/OpenRefine/wiki/Reconciliation-Service-API) developed by the same team. One feature copied from the reconciliation API is ``queries``, a wrapper list to support bulk requests in the future. The dictionary inside of it is a simple MQL query.

A query using MQL could also make use of link structures within the Popolo data, if the backend supports such queries:

```json
{
    "queries": {
        "q0": {
            "type": "popolo:Person",
            "memberships": {
                "organization": {
                    "name": "Big Corp",
                    "type": "popolo:Organization"
                },
                "*": null
            },
            "*": null,
            "limit": 5,
        }
    }
}
```

This attempts to find all persons who are members of an organization with the given name. Because of the wildcard queries (``*``), all properties of the person and membership will be returned.

### Response format

A simple response would return the requested entity information in Popolo format, with sources information on a per-record level included:

```json
{
    "queries": {
        "q0": {
            "hits": 234,
            "result": [
                {
                    "id": "http://dirtlist.com/companies/us_de/123456",
                    "identifiers": [
                        {
                            "identifier": "123456789",
                            "scheme": "http://dirtlist.com/companies/us_de"
                        },
                        {
                            "identifier": "123456789",
                            "scheme": "DUNS"
                        }
                    ],
                    "name": "Big Corporation",
                    "score": 14.601182,
                    "sources": [
                      {
                        "url": "https://delecorp.delaware.gov/eCorp/LoginAnnualReportsCLF"
                      }
                    ]
                },
                [...]
            ]
        }
    },
    "status": "200 OK"
}
```

Given the diverse nature of data providers and use cases, the API supports different response detail levels:

* Full data resonses will return a structured data record or set of records to match the query.
* Reference lists return data maintained by the service may not be fully structured (such as relevant plain-text documents). The returned information is limited to document references which can be accessed by a human researcher.
* Information holder contact, in scenarios where the result data itself cannot be shared, contact information for the person holding the relevant material should be indicated.

### API conventions

* All requests are sent as JSON queries over HTTP. They can use either the GET method (and a ``query=`` argument) or the request body of a POST request. Responses are assumed to be formatted in JSON, even if the result is an error.
* Data providers are free to supply links to other resources on their platform, outside of the existing request endpoint. 
* All of the proposed API will rely on the Popolo specification and Freebase MQL as far as possible, and any further additions, extensions and standards will be documented in this repository.

### Data model extensions

While Popolo provides a data model for ``Persons``, ``Organizations`` and ``Memberships``, it is mainly structured around parliamentary information use cases.

It does not currently have a notion of ``Companies``, nor does it define relevant connection types such as ``Control`` (e.g. for ownership) and ``Transaction`` (e.g. for contracts). For a more detailed analysis of potential connection types, see [James' inventory of terms](https://docs.google.com/spreadsheets/d/1on99aF9QVWOwqZDtla9RLX5Wza6MHxbb4apZwbtHK-w/edit#gid=917587019).  

## Getting it adopted

* Build a set of proxies around existing data providers (i.e. partners, but also ICIJ Offshore, CorpWatch CrocTail - even WhoIs)
* Prototype client applications around different patterns of use
    
### Who should be involved?

* Partners of Influence Mappers (OpenCorporates, Poderopedia, LittleSis, OpenNorth, American Academy)
* International Consortium of Investigative Journalists and the Organized Crime and Corruption Reporting Project 
* Consumer-side apps: detective.io, kumu.io, linkurio.us
* Producer-side apps: Overview, DocumentCloud, PopIt, Investigative Dashboard, CrocTail

### Existing efforts

* [Popolo](http://www.popoloproject.com/) data standard for people and organizations.
* Linked Data, [Linked Data Fragments](http://linkeddatafragments.org/), FOAF, [SPARQL](http://www.w3.org/TR/rdf-sparql-query/)
* OpenRefine [Reconciliation API](https://github.com/OpenRefine/OpenRefine/wiki/Reconciliation-Service-API) for fuzzy entity matching.
* [Metaweb Query Language](http://wiki.freebase.com/wiki/MQL), a more sophisticated query mechanism which supports matching entities in terms of their connections to other nodes in a graph. Also: [CYPHER](http://neo4j.com/developer/cypher-query-language/).
* Survey of [partner project data models](https://docs.google.com/spreadsheets/d/1on99aF9QVWOwqZDtla9RLX5Wza6MHxbb4apZwbtHK-w/edit).
* [IATI OrgID](http://iatistandard.org/201/codelists/OrganisationRegistrationAgency/) scheme for company identification.
* [OpenCorporates API](https://api.opencorporates.com/), [LittleSis API](http://api.littlesis.org/).
