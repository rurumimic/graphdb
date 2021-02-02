# Movies

## Cypher

a graph query language. to query the Neo4j Database.

```sql
MATCH (m:Movie) WHERE m.released > 2000 RETURN m LIMIT 5
MATCH (m:Movie) WHERE m.released > 2005 RETURN m
MATCH (m:Movie) WHERE m.released > 2005 RETURN count(m)
```

![](http://guides.neo4j.com/sandbox/movies/img/5-movies.svg)

## Nodes and Relationships

- `(n:Node)`: entity. similar to a row in a rdb.
  - n: variable
  - Node: type of node

![](http://guides.neo4j.com/sandbox/movies/img/schema.svg)

- `[r:Relationship]`: connecting the corresponding types of nodes.
  - r: variable
  - Relationship: type of relationship

```sql
MATCH (p:Person)-[d:DIRECTED]-(m:Movie) WHERE m.released > 2010 RETURN p,d,m
MATCH (p:Person)-[d:ACTED_IN]-(m:Movie) WHERE m.released > 2010 RETURN p,d,m
```

![](http://guides.neo4j.com/sandbox/movies/img/movies-after-2010.svg)

## Labels

`:Label`: a name or identifier of a Node or a Relationship.

```sql
MATCH (p:Person) RETURN p LIMIT 20 -- only Person nodes
MATCH (n)        RETURN n LIMIT 20 -- all kinds of nodes
```

## Properties

name-value pairs. to add attributes to nodes and relationships.

```sql
MATCH (m:Movie) RETURN m.title, m.released, m.tagline
MATCH (p:Person) RETURN p.name, p.born
```

![](http://guides.neo4j.com/sandbox/movies/img/movies-properties.jpg)

## Create a Node

```sql
CREATE (p:Person {name: 'John Doe'}) RETURN p
MATCH (p:Person) WHERE p.name = 'John Doe' return p
```

## Finding Nodes with Match and Where Clause

```sql
MATCH (p:Person {name: 'Tom Hanks'}) RETURN p
MATCH (p:Person) WHERE p.name = 'Tom Hanks' RETURN p
MATCH (m:Movie {title: "Cloud Atlas"}) RETURN m
MATCH (m:Movie) WHERE m.released > 2010 AND m.released < 2015 RETURN m
```

## Merge Clause

**Usage:**
1. match the existing nodes and bind them
1. create new node(s) and bind them

### ex 1

- Create the Person node if it does not exist
- If the node already exists, then it will set the property `lastLoggedInAt` to the `current timestamp`
- If node did not exist and was newly created instead, then it will set the `createdAt` property to the `current timestamp`

```sql
MERGE (p:Person {name: 'John Doe'})
ON MATCH SET p.lastLoggedInAt = timestamp()
ON CREATE SET p.createdAt = timestamp()
RETURN p
```

### ex 2

- Create a movie node with title "Greyhound"
- If the node does not exist then set its `released` property to `2020` and `lastUpdatedAt` property to the `current timestamp`
- If the node already exists, then only set `lastUpdatedAt` to the `current timestamp`. 
- Return the movie node

```sql
MERGE (m:Movie {title: 'Greyhound'})
ON MATCH SET m.lastUpdatedAt = timestamp()
ON CREATE SET m.released = 2020, m.lastUpdatedAt = timestamp()
RETURN m
```

## Create a Relationship

```sql
MATCH (p:Person), (m:Movie)
WHERE p.name = "Tom Hanks" AND m.title = "Cloud Atlas"
CREATE (p)-[w:WATCHED]->(m)
RETURN type(w)
```

`type(w)`: `WATCHED`

```sql
MATCH (p:Person), (m:Movie)
WHERE p.name = "John Doe" and m.title = "Cloud Atlas"
CREATE (p)-[w:WATCHED]->(m)
RETURN type(w)
```

## Relationship Types

- incoming: Tom Hanks
- outgoing: Cloud Atlas

![](http://guides.neo4j.com/sandbox/movies/img/relationship-types.svg)

```sql
MATCH (p:Person)-[r:ACTED_IN]->(m:Movie) RETURN p,r,m
MATCH (p:Person)-[r:ACTED_IN]-(m:Movie) RETURN p,r,m
MATCH (p:Person)-[r:REVIEWED]-(m:Movie) RETURN p,r,m
```

## Advance Cypher queries

Finding who directed Cloud Atlas movie:

```sql
MATCH (m:Movie {title: 'Cloud Atlas'})<-[d:DIRECTED]-(p:Person) RETURN p.name
MATCH (p:Person)-[d:DIRECTED]->(m:Movie {title: 'Cloud Atlas'}) RETURN p.name
```

Finding all people who have co-acted with Tom Hanks in any movie:

```sql
MATCH (tom:Person {name: "Tom Hanks"})-[:ACTED_IN]->(:Movie)<-[:ACTED_IN]-(p:Person) RETURN p.name
MATCH (:Person {name: "Tom Hanks"})-[:ACTED_IN]->(m:Movie)<-[:ACTED_IN]-(p:Person) return m.title,p.name
```

Finding all people related to the movie Cloud Atlas in any way:

```sql
MATCH (p:Person)-[relatedTo]-(m:Movie {title: 'Cloud Atlas'}) RETURN p.name,type(relatedTo)
```

Finding Movies and Actors that are 3 hops away from Kevin Bacon:

```sql
MATCH (p:Person {name: 'Kevin Bacon'})-[*1..3]-(hollywood) return DISTINCT p, hollywood
```
