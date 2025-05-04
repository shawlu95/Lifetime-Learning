### Meet Solr

- document oriented: no nested field, doesn't depend on other doc, renormalized relations
- flexible schema: text-centric, read-dominant, document-oriented, flexible
  - copy field: take the raw text of one field, apply to a different field
  - dynamic field: apply same field type to many different field without declaring
- multiple indexes in one server: each core is a separate index and config
- result grouping/field collapsing: put multiple docs in to group; return unique groups instead of individual docs
- join: return child docs of parents that match search criteria
- clustering: pick a single representative from a cluster

### Getting Started

- solr is a web app runnng in Jetty (a Java web server)
- only **one** solr home directory per Jetty server (`solr.solr.home`)
- host multiple cores per server, each core with a saparate index and config under solr home
- http://localhost:8983/solr/#/
- query console components:
  - q: main query
  - fq: filter query
  - sort
  - start
  - row
  - fl: list of fields to return
  - df: default search field
  - wt: response write type
- browse collection: http://localhost:8983/solr/collection1/browse

```bash
cd $SOLR_INSTALL/example
java -jar start.jar

# start on a different port
java -Djetty.port=8080 -jar start.jar

# ctrl-c to stop
```

In a separate terminal, index documents

```bash
cd $SOLR_INSTALL/example/exampledocs
java -jar post.jar *.xml
```

Example HTTP Get request

```
http://localhost:8983/solr/collection1/select?q=iPod&fq=manu:Belkin&sort=price&fl=name,price,features,score&df=text&wt=xml&start=0&rows=10

```

#### Create a new home

- Create a deep copy of the example/ directory; for example, cp -R example realestate.
- Clean up the cloned directory to remove unused Solr home directories, such as example-DIH/ and multicore/; they’ll be in example/ if you need to refer back to them.
- Under the Solr home directory, rename collection1/ to something more intuitive for your application.
- Update core.properties to point to the name of your new collection by replac- ing collection1 with the name of your core from step 3; such as: name =realestate.

```bash
# stop the running solr with ctrl-c
cd $SOLR_INSTALL/passport
java -jar start.jar
```

#### Syntax

- default space means `OR`
- while card: `*` match zero of more character, `?` match exactly one
- range search inclusive: `age:[18 TO 21]`, exclusive `{18 TO 21]`
- edit distance (apply to character): `administrator~2` matches two edit distance
- proximity search: `"chief officer"~1` matches chief operating officer
