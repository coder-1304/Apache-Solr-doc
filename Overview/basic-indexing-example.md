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
 
## Basic Searching
- Solr can be queried via REST clients, curl, wget, Chrome POSTMAN, etc., as well as via native clients available for many programming languages.
- If you click the Execute Query button without changing anything in the form, you’ll get 10 documents in JSON format by default
  ```http
  http://localhost:8983/solr/techproducts/select?indent=on&q=*:*
  ```
- Here, we are using Solr’s query parameter (q) with a special syntax that requests all documents in the index (*:*). All of the documents are not returned to us, however, because of the default for a parameter called rows, which you can see in the form is 10.
- Add parameter `&rows=n` to fetch `n` rows

### Search for a single term
- Replace `q=*:*` with the term you want to find. e.g. `q=foundation`
  
### Multi/phrase search
- To search for different terms, replace foundation with:
  - 32MB
  - "SD CARD"
  - "USB Cable"
- It means we want to search for the entire string

### Field searches
So far all queries were about searching in all fields that is done till now. But if we want to search for a specific field, we use:
- `q=[field]:[value]`
- For example `q=cat:electronics`, it means we want to search for field "cat" and its value should be "electronics"

### Combining searches
To find documents that contain both terms "electronics" and "music", we use- `q= +electronics -music`:
- `+electronics` means include electronics word
- `-music` means exclude music word

## Delete Collection
```bash
  bin/solr delete -c <Collection Name>
```



