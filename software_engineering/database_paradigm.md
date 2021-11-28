
## Database Paradigm

1. Key-value Pair
  * redis, memcache
  * data in RAM, very fast, but lightweight
  * usually a cache layer on some other db
2. Wide Column
  * cassandra, HBase
  * each column family holds a set of ordered rows
  * schemaless. handle unstructured data
  * CQL instead of SQL cannot do join
  * horizontal scaling
3. Document
  * MongoDB, FireStore, DynamoDB, CouchDB
  * a doc is a container of key-val pair
  * no join, de-normalize data: read fast, complex write
4. Relational:
  * MySQL, PostgreSQL
  * SQL: structured query language
  * organize data in smallest normal forms, which requires strict schema
  * ACID compliant, good for banking, trading
5. Graph
  * Neo4J, DGraph
  * use cases: finance fraud detection, recommendation engine
6. Search
  * elasticsearch, Lucene, Solr
  * similar to doc-oriented database, except for inverted index
    - handle typo
    - score by relevance
7. Multi-model: Fauna DB
  * describe how you want to access data using graphQL
  * Fauna determines how best to combine different paradigm
  * ACID compplient
