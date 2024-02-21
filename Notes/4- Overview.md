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
- Then you have to enter name of your collection. Enter a name and press enter
- Then it will ask how many shards you want to create. Go with default [2] and press enter
- Then it will ask how many replicas you want to create. Again go with default.
- Then for configuration go with default and press enter

This will start you Solr in SolrCloud Mode. Go to - http://localhost:4000/solr to open admin page

