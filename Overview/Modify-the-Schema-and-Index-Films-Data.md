# Creating a new collection

**Field Guessing**: Solr attempts to guess what type of data is in a field while its indexing it. It also automatically creates new fields in the schema for new fields that appear in incoming documents. This mode is called "Schemaless"

**Schema**: Solr's schema is a single file (in XML) that stores the details about the fields and field types solr is expected to undetstand.

## Restarting solr

- To start first node, this command is used
  ```bash
    ./bin/solr start -c -p 8983 -s example/cloud/node1/solr
  ```
- When it's done start second node
  ```bash
    ./bin/solr start -c -p 7574 -s example/cloud/node2/solr -z localhost:9983
  ```

## Creating a new collection

```bash
  bin/solr create -c films -s 2 -rf 2
```
- `-s 2` : Number of shards
- `-rf 2`: Specifies replication
- We didn't specify a configset, so "_default" will be used

## Preparing schemaless for the films data
Field guessing is designed to allow us to start using solr without having to define all the fields we think will be in our documents before trying to index them. That's why it is called schemaless because we can start quickly and let solr create fields for you as it encounters them in document
**Limitations**: If it guesses wrong, you can't change much about a field after data has been indexed without having to reindex.


```json
{
  "add-field": {
    "name": "name",
    "type": "text_general",
    "multiValued": false,
    "stored": true
  }
}
```
