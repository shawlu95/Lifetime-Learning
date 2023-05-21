#### Start Solr

- Web admin: `http://localhost:8983/solr`

```bash
cd $SOLR_INSTALL/example
java -jar start.jar

# kill process on port
lsof -i:8080
```

Formulating HTTP request ([tutorial](file:///Users/shawlu/solr-4.7.0/docs/tutorial.html))

- q: main query
- fq: filter query
- sort
- fl: list of fields to return
- df: default search field
- wt: return format (xml, json etc)

```
http://localhost:8983/solr/collection1/select?q=iPod&fq=manu:Belkin&sort=price asc&fl=name,price,features,score&df=text&wt=xml&start=0&rows=10
```

#### Faceted search

- Count documents for each facets, allow user to refine queries and drill down. Similr to `count (*) ... group by ...`
- Can specify multiple facet fields
- Facet queries `facet.query=`
- Can facet by numeric range, specify start, end, interval

---

### Chapter 4

#### solrconfig.xml

- specify a lucene version on which solr is based
- **lib** directory contains JAR files
  - use regex or exact path
- use **listener** to register event handler

#### Nested Data Structure

- **arr** named, ordered values (no key)
- **lst** named, ordered, key-value pair

#### Learn by example

- see default params: `http://localhost:8983/solr/collection1/browse?q=iPod&wt=xml&echoParams=all`
- stats: `http://localhost:8983/solr/collection1/select?q=*:*&wt=xml&stats=true&stats.field=price`
- debug: `http://localhost:8983/solr/collection1/browse?q=iPod&wt=xml&debug=true`

#### Search Handler

- request parameters decoration
  - default: can be overwritten
  - invariant: cannot be overwritten (override client params)
  - append: to be combined with client's param
- first-components: preprocessing
- components: must at least include the query component
- last-components: post-processing

#### Searcher

A searcher is a read-only snapshot of underlying Lucene index

- **commit** means closing current searcher and opening a new one
  - Before destroying old searcher, must wait for current queries to complete
  - Must invalidate all cache based on old searcher
- only one searcher is active at a time.
- new searcher needs to be warmed up
  - specify warming queries manually
  - warming queries are executed to populate new searcher's cache
  - `USECOLDSEARCHER`: immediately register warming searcher as active
  - `MAXWARMINGSEARCHERS`: limit number of warming searcher, which eat up RAM

#### Cache

- Two policies: LRU cache (default), least frequently used cache (for filter cache)
- hit ratio measures efficiency of cache
- All caches are linked to specific instance of searcher. New searcher invalidates old cache
- filter cache: reusable across different queries; doesn't affect scoring
  - key is filter query
  - during auto-warming, key is executed on new searcher
- query result cache:
  - key is query, values is set of doc ID
  - can limit max number of doc per query
  - suggest lazy loading, if doc has too many fields
- document cache
  - query cache still needs to look up documents from disk by ID
  - valuable if documents are most often static
- field value cache: managed by Lucene, not Solr

---

### Chapter 5 Indexing

- A document is made of of fields
- A field has a type that decides how it's stored, searched, analyzed
- Each doc has an unique key, or `id` field. Reindex will replace any existing doc with same `id`
- indexed fields: users will most likely search by the fields, or sort, filter, apply transform
- stored fields: useful in display results, but not used when searching
- No nested fields allowed. All fields are siblings
- For array fields, set `multiValued="true"`

#### How Solr Indexes documents

1. Transform doc to a Solr-compatible format (xml)
2. Apply tokenizer and filters (text analysis)
3. Write processed doc to index

#### Step in Designing Index

1. Choose the ID field: to avoid duplication
2. Choose indexed fields: depending on how users are expected to search
3. Select stored fields: add only when necessary. The **original value** is stored (not analyzed value)

#### Schema.xml

- `fields`: define field names, types. Each field as a name that query can refer to
- `uniqueKey`
- `dynamicField`:
  - user glob-pattern (wild card) to define multiple fields
  - when indexing, can add new field without modifying schema file
  - at query time, still need to specify complete field name, not glob pattern
- `copyField` most ofen used as a "catch-all" field that is indexed but not stored
  - create a multi valued field in `fields` section
  - create one `copyField` entry for each field that needs to be copied to the multi-valued field destination
  - the raw value of source field is copied to destination field, and then analyzed according to the destination field's type
- `types`: how each type is analyzed (at indexing & query time)
  - string type is not altered
  - date field is based on trie
  - numeric field can specify `precisionStep` to balance efficiency and space
    - smaller `precisionStep` generates more terms, cost more space, leads to faster query

#### Commit

- hard commit (normal commit): changes are flushed to disk. Requires opening a new **searcher**. Can be blocking if set `waitSearcher=true`
- soft commit: changes are available for search, but not yet flushed to disk. Need hard commit sometime later
- transaction log: a new log is opened whenever a hard commit is issued

#### Atomic Update

- To update a document, existing doc is deleted, and fields pulled into new doc
- **optimistic concurrency**: uses a `_version_` field.
  - before sending an update, first read current version, which is attached to post request
  - update will fails if request' version doesn't match current index version

---

### Chapter 6

How Solr Understand text:

- language-specific parsing
- part of speech tagging
- lemmatization: unify inflectional forms such as organize vs organise
- stemming: remove `-ing ` (`KStemFilterFactory`)
- lower-case transform (`LowercaseFilter`)
- remove stop words (google patent: www.google.com/patents/US7945579)
- remove astrophes, hyphen (`WordDelimiterFilter`)
- remove accents, diacritical marks `solr.ASCIIFoldingFilterFactory`
- remove repeated letters (`PatternReplaceCharFilterFactory`)
- preserve hashtag and mention (`WordDelimiterFilterFactory`)
- inject synonyms (`SynonymFilterFactory`)

The above mentioned transform can be configured in schema file. No java code is involved!

- In schema file, define **two analyzers**: how documents are indexed, and how queries are analyzed

How an analyzer is constructed:

- char filter (optional)
  - apply regex replacement before tokenization
- tokenizer (first) does one of the three (order is important):
  1. transform
  2. injection
  3. removal
- token filters (second)
  - synonym is processed on query, not indexing

#### Extending a Plug-in

Applicable when need to write custom java code for a tokenizer, token filter.

1. Extend the `TokenFilter` class
2. Extend the `TokenFilterFactory` class, which parses xml params
3. Place the JAR file containing plugin class

Query and filters:

- `q`: a query, returns a subset of docs, and list of terms for each doc for calculating relevance score
- `fq`: filter query, only return a subset of docs

Order of execution

- execute filter query, either in cache or in index
- execute query, result is intersected with filter query result
- applt post filters (most expensive)

Order filters

- execute expensive filters last, cheap filters first
- execute filters that reduces most documents first

---

### Chapter 7

Request handler: receive a request, perform some action, return resposne to client

- name starts with `/` and is part of the URL
- search handler is the most important request handler
  - contains multiple search component
  - query component is the most important component that executes the query
- there also update request handler, more like this handler etc

#### Lucene query parser

- fielded search term: `title:solr`
- un-fielded search uses `df` default field: `solr`
- grouped expression using doule quote `title: "solr in action"`
- required term uses `+` or `AND` or `&&`
- optional term: `||` or `OR` or space e.g. `solr in action`
- negate term uses `-` or `NOT`
- term proximity: `"solr in action"~3`
- character proximity: `Lucene~3`
- range searches
  - `age:[18 to 30}`: 18<=age<30
  - `age:[18 to *]`: 18<=age
- boost expression: `(apache^10 solr^100 is^0 awesome^1.234) AND (apache lucene^2.5)^10`
- special character
  - use group expression, or use `\` to escape
  - stripe all special character from user input, before sending to solr
