# Graph DB

[Wikipedia](https://en.wikipedia.org/wiki/Graph_database)

- Use **graph structures** for **semantic queries** with **nodes**, **edges**, and **properties** to represent and store data.  
- Querying relationships is fast because they are perpetually stored in the database.  
- **Relationships** are a **first-class citizen** in a graph database and can be labelled, directed, and given properties.
- Network-model databases operate at a lower level of abstraction and lack easy traversal over a chain of edges.
- Gremlin, SPARQL, and Cypher. most often tightly tied to one product.

## Background

A graph database is a database that is based on [graph theory](https://en.wikipedia.org/wiki/Graph_theory). 

- Node: entities or instances. roughly the equivalent of a record, relation, or row in a relational database, or a document in a document-store database.
- Edge: graphs or relationships. the lines that connect nodes to other nodes. not directly implemented in a relational model or a document-store model.
- Property: information associated to nodes.

## Graph models

### Labeled-property graph

- a set of nodes, relationships, properties, and labels.
- Both nodes of data and their relationships are **named** and can **store properties** represented by **key/value pairs**. 
- Relationships can also have properties.

### Resource Description Framework

- the addition of information is each represented with a separate node.
- composed of nodes and arcs. 
- Node: blank / literal(uptyped/typed) / URI
- Arc: URI

## Properties

- insert new data without loss of application functionality
- no need to plan out extensive details of the database's future use cases

### Storage

- relational engine
- key-value store
- document-oriented database

### Index-free adjacency

- Data lookup performance is dependent on the access speed from one particular node to another.
- sacrifices the efficiency of queries that do not use graph traversals.

### Graph types

- Social graph: this is about the connections between people
- Intent graph: this deals with reasoning and motivation
- Consumption graph: payment graph. heavily used in the retail industry.
- Interest graph: maps a person's interests. 
- Mobile graph: this is built from mobile data. from the web, applications, digital wallets, GPS, and IoT devices.

## Comparison with relational databases

**RDB**

- relational models require a strict schema and data normalization imposed limitations on how relationships can be queried.
  - data is normalized to support ACID transactions
  - removes any duplicate
  - to preserve data consistency
- design motivations was to achieve a fast row-by-row access
- complex queries, many join operations

**Graph DB**

- often faster for associative data sets
- map more directly to the structure of object-oriented applications
- marketed as more suitable to manage ad hoc and changing data with evolving schemas

**Conclusion**

- RDB: typically faster at performing the same operation on large numbers of data elements
  - very well suited to flat data layouts, where relationships between data is one or two levels deep.
- Graph DB: if there is an evidence for performance improvement by orders of magnitude and lower latency
  - well suited to social networking systems
  - suited to types of searches that are increasingly common in online systems, and in big data environments.

### Example

- `people`: `person_id`, `person_name`
- `friend`: `friend_id`, `person_id`

Search all of Jack's friends:

```sql
SELECT p2.person_name 
FROM people p1 
JOIN friend ON (p1.person_id = friend.person_id)
JOIN people p2 ON (p2.person_id = friend.friend_id)
WHERE p1.person_name = 'Jack';
```

Cyper:

```sql
MATCH (p1:person)-[:FRIEND-WITH]-(p2:person)
WHERE p1.name = "Jack"
RETURN p2.name
```

SPARQL:

```sql
PREFIX foaf: <http://xmlns.com/foaf/0.1/>

SELECT ?name
WHERE { ?s a          foaf:Person . 
        ?s foaf:name  "Jack" . 
        ?s foaf:knows ?o . 
        ?o foaf:name  ?name . 
      }
```

```sql
SELECT people.name
FROM (
       SPARQL PREFIX foaf: <http://xmlns.com/foaf/0.1/>
              SELECT ?name
              WHERE { ?s foaf:name  "Jack" ; 
                          foaf:knows ?o .
                      ?o foaf:name  ?name .
                    }
    ) AS people ;
```

---

## Documentation

### Neo4j

- [developer](https://neo4j.com/developer/)
  - [Get started](https://neo4j.com/developer/get-started)
  - [GraphAcademy](https://neo4j.com/graphacademy)
- [Cypher](https://neo4j.com/docs/cypher-manual/current/)
  - [Refcard](https://neo4j.com/docs/cypher-refcard/current/)

### Amazon Neptune

- [Amazon Neptune](https://aws.amazon.com/neptune)

---

## Tutorials

neo4j: [sandbox](https://sandbox.neo4j.com)

- [Movies](tutorials/movies/README.md)
- [Crime Investigation](tutorials/crime-investigation/README.md)
- [Recommendations](tutorials/recommendations/README.md)