# Understanding Solr Indexing

Solr utilizes an index to efficiently retrieve information from stored documents. This index performs mapping words, terms, or phrases to their respective locations within the documents stored in Solr.

## How Solr Indexing Works

Solr indexing is the process of adding documents to the Solr search index so that they can be searched and retrieved later. When a document is indexed, Solr analyzes its contents and creates an index that maps each word or term to the documents that contain it. This index is then used to quickly find documents that match a given query.

For example, if you're searching for the term "laptop" Solr looks up this term in its index to quickly find the relevant documents without having to re-read the entire text of each document.

When a document containing 100 words is indexed in Solr, each of the 100 words is indexed separately. This means that there will be 100 separate indexes, each mapping a word to the documents that contain it. This indexing approach allows for precise and efficient search functionality, enabling Solr to quickly retrieve documents based on the terms they contain.

## Indexing Steps

The indexing process typically involves the following steps:

1. **Document Parsing:** Solr reads the documents to be indexed and extracts their contents, which may include text, metadata, and other data.

2. **Analysis:** Solr analyzes the contents of the documents to identify individual terms and phrases, and applies various text processing techniques to normalize and enhance them.

3. **Indexing:** Solr creates an index that maps each term to the documents that contain it, along with information about the term's frequency, position, and other properties.

4. **Committing:** Once the indexing process is complete, Solr commits the changes to the index, making them available for search.


## Fields in Solr Index

Solr uses the concept of fields to organize and index documents. A field represents a specific type of information within a document, such as title, author, or content. Each field in Solr's index contains the indexed information along with additional metadata to facilitate efficient searching and retrieval.

For instance, if you have a document with fields like "title," "author," and "content," Solr will index each field separately, allowing for targeted searches within specific fields.

In summary, Solr's indexing mechanism optimizes search performance by creating a structured index of words, terms, or phrases, enabling quick and efficient retrieval of relevant documents based on user queries.

## Solr Cloud

SolrCloud is a distributed search platform built on top of Apache Solr, designed to provide scalability, fault tolerance, and centralized configuration management. It enables you to deploy Solr across a cluster of machines, distributing indexes and queries for high availability and performance. 

**Key components include** 

- **Cores** : Cores are fundamental units in Solr, representing individual indexes. Each core handles indexing and searching operations for a specific set of data. It contains configuration files, index data, and other resources necessary for search functionality. A core in Solr represents a separate index with its own configuration and schema.

- **Collections** : Collections are logical groupings of cores managed together as a single unit within SolrCloud. They allow for scalability and distributed search capabilities. Collections can span multiple Solr nodes and are typically used to partition data across multiple cores for efficient processing.

- **Sharding** : Refers to a subset of a larger index. When an index becomes too large to handle on a single server, it is divided into smaller, more manageable pieces called shards. Each shard contains a portion of the index's data and can be stored on a separate server or node within a SolrCloud cluster. Sharding helps improve performance, scalability, and fault tolerance by distributing the index across multiple servers, allowing for parallel processing of queries and better resource utilization.

- **Solr Nodes** : Solr nodes are individual instances of the Solr server running on separate machines within a SolrCloud cluster. Each Solr node manages one or more collections of indexed data and handles search requests from clients. Solr nodes communicate with each other and with ZooKeeper for cluster coordination, ensuring that the search index remains consistent across the cluster.

- **ZooKeeper** : ZooKeeper is a centralized service for maintaining configuration information, providing distributed synchronization, and managing cluster state for distributed systems like SolrCloud. In a SolrCloud deployment, ZooKeeper manages cluster coordination tasks such as leader election, shard allocation, and tracking the status of individual Solr nodes. It stores metadata about collections, shards, and replicas, enabling Solr nodes to dynamically join or leave the cluster without affecting its availability or data integrity.

- **Replicas** : Replicas in Solr are duplicate copies of a shard's data distributed across nodes for fault tolerance and high availability. They enable load balancing and parallel query processing in SolrCloud, ensuring resilience against node failures and scaling for large search traffic.

### Launching Solr in SolrCloud Mode

To start Solr in SolrCloud mode, use the following command:
```bash
bin/solr start -e cloud
```
This will start an interactive session that will start 2 solr servers on machine

- `./bin/solr` refers to solr script located in the directory
- `start` means start solr instance
- `-e cloud` starts solr instance in cloud mode
  - In solr cloud mode, a solr cluster consist of multiple solr nodes working together to handle search queries and index updates

Follow up process:

- This will ask *how many Solr nodes would you like to run in your local cluster?*. Press enter to run 2 nodes which is default (Default option is shown in []).
- Now you have to enter ports for both nodes. Press enter to use default port.
- Then you have to enter name of your collection. Enter a name for your collection and press enter
- Then it will ask how many shards you want to create. Go with default [2] and press enter
- Then it will ask how many replicas you want to create. Again go with default.
- Then for configuration go with default and press enter

This will start you Solr in SolrCloud Mode. Go to - http://localhost:8983/solr to open admin page.

### Creating a new collection and indexing data

After creating a collection you have to index some data into it. 

Note - This doc contains basic information of indexing and searching. The next doc that contain actual exercise for hands on.

You can index sample data using the `bin/post` tool provided by Solr:
```bash
bin/post -c <Your Collection Name> <Path To Document(s)>
```
- Your Collection Name: Enter the collection name that you have created previously.
- Path To Document(s) : Give the location of the documents in the directory. use `/*` to include all the files in a directory.

This command will index various types of documents (JSON, XML, CSV etc.) included in the your directory.

# Solr Basic Searching

Solr can be queried through various methods including REST clients, curl, Chrome POSTMAN, etc., or through native clients available for many programming languages. The Solr Admin UI provides a query builder interface via the Query tab for collections. Go to admin UI and in the sidebar you will see a Collection Selector. Click on your collection here. This will list down the options. Click on the Query option. It will show the panel where you can query the data that you have indexed.

You can also go directly via this link- http://localhost:8983/solr/#/mycollection/query (replace mycollection with the name of collection that you have created)
![image](https://github.com/coder-1304/Apache-Solr-doc/assets/121802518/890aa573-da9d-4ddd-9205-bae75c4df441)

Click on the `Execute query` without selecting any option. This will list down all of the data that you have indexed. But in the response you will see only 10 results because by default maximum 10 rows are returned via this query. To return `n` number of rows add this parameter to the end of the url `&rows=n` to retrieve maximum `n` number of rows

## Performing Searches
### Query Parameters
- **q**: Represents the query parameter used to specify search terms.
- **rows**: Determines the number of rows (documents) to be returned in the search results.

### Combining Searches
You can search for multiple terms or phrases in a single query via `q="term1 term2 term3"`
Also you can require or allow a phrase using `+` prefix in a term and `-` prefix to disallow a term

### Example Queries
- Searching for all documents: `q=*:*`
- Searching for a single term: `q=foundation`
- Searching for documents within a specific field: `q=cat:electronics`
- Searching for a multi-term phrase: `q="CAS latency"`
- Searching for documents containing *electronics* phrase and disallow *music* phrase`q="+electronics -music"`

(We'll cover this in detail in exercises)

## Delete a collection
To delete a collection use this command
```bash
bin/solr delete -c <Collection Name>
```

## Alternative to create a collection
You can directly create a collection using this command
```bash
bin/solr create -c <collection name> -s 2 -rf 2
```
- `-s 2` : Means we want to create 2 shards
- `-rf 2`: Specifies replication, 2 in this case

In this command we didn't mention anything about a configset. That's okay though because Solr knows how to handle this. It automatically uses a default configuration set called "_default" when you don't specify one explicitly. This "_default" configset is well-suited for most general use cases, hence its name. So even though we didn't mention it, Solr knows what to do behind the scenes.


## Closing Solr instance

This command stops all running Solr nodes on the local machine:
```bash
bin/solr stop -all
```

Alternatively, you can specify a specific node to stop by providing its port number:
```bash
bin/solr stop -p <port_number>
```
Replace <port_number> with the port number of the Solr instance you want to stop.

## Restart Solr
if you've stopped you need to restart it, follow these steps:

1. Start the first node using the following command:
    ```bash
    ./bin/solr start -c -p 8983 -s example/cloud/node1/solr
    ```
    This command initiates the first node.

2. Once the first node is up and running, proceed to start the second node and specify its connection to ZooKeeper:
    ```bash
    ./bin/solr start -c -p 7574 -s example/cloud/node2/solr -z localhost:9983

## Solr Schema
In Solr, the schema serves as a crucial blueprint stored in a single XML file, outlining the structure and characteristics of fields and field types that Solr recognizes and manages. Essentially, it's a roadmap for Solr to understand and process data. The schema not only defines the names of fields and field types but also includes instructions on how to preprocess data before indexing it. For instance, if you want variations like "abc" and "ABC" to match when searching, you can specify in the schema to normalize such terms, perhaps by converting them to lowercase during indexing and querying. Additionally, the schema allows for the creation of copy fields, which aggregate data from other fields, and dynamic fields, which are generated based on wildcard patterns.



