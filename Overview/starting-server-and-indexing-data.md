# Starting server
## Command to start the server
  ```bash
    ./bin/solr start -e cloud
  ```
This will start an interactive session that will start 2 solr servers on machine

- `./bin/solr` refers to solr script located in the directory
- `start` means start solr instance
- `-e cloud` starts solr instance in cloud mode
  - In solr cloud mode, a solr cluster consist of multiple solr nodes working together to handle search queries and index updates

**Zookeeper**: Centralized service for maintaining configuration information in a distributed system.
- If we don't define any external zookeper in solr cloud mode, solr launches its own zookeeper and connects both nodes to it.

## Creating collection
- After creating 2 nodes in solr we have to name our collection then press enter
- It will ask how many shard you want to create
- Shard is a logical partition of the collection, containing a subset of documents from the collection, such that every document in a collection is contained in exactly one shard.
- Choosing "2" (the default) means we will split the index relatively evenly across both nodes, which is a good way to start. Then press enter
- Configset contains essential configuration fields required by solr, such as the schema file (either managed-schema or schema.xml)

## Indexing data
- Solr includes the `bin/post` tool in order to facilitate indexing (posting) various types of documents easily
- We can index all formats of data (JSON, CSV, XML etc) all at once
- Command to index data:
  ```bash
    bin/post -c techproducts example/exampledocs/*
  ```
  - `-c techproducts` option specifies the name of the solr collection where the documents should be indexed
  - `example/exampledocs/*` is the path where data is stored that we have to index
