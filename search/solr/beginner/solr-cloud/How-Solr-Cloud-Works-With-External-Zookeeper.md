# Understanding SolrCloud

![SolrCloud](https://miro.medium.com/v2/resize:fit:1100/format:webp/1*Y85rcaIsa9BgVqb1cLRAlg.jpeg)

Image source: [Setting up Solr Cloud 8.4.1 with Zookeeper 3.5.6](https://medium.com/@sarkaramrit2/setting-up-solr-cloud-6-3-0-with-zookeeper-3-4-6-867b96ec4272)


## Introduction
In SolrCloud, a distributed search and indexing system, there's no single leader node responsible for allocating resources like nodes, shards, and replicas. Instead, Solr relies on ZooKeeper to manage these aspects based on configuration files and schemas. This setup allows queries and updates to be sent to any server within the SolrCloud cluster. Solr utilizes the information stored in ZooKeeper to determine which servers need to handle a particular request.

## Query Routing
In SolrCloud there are no masters or slaves. Instead, every shard consists of at least one physical replica, exactly one of which is a leader. Leaders are automatically elected, initially on a first-come-first-served basis, and then based on the ZooKeeper process.

When a document is sent to a Solr node for indexing, the system first determines which Shard that document belongs to, and then which node is currently hosting the leader for that shard. The document is then forwarded to the current leader for indexing, and the leader forwards the update to all of the other replicas.

## Shard and Replica Management
In SolrCloud, each shard comprises at least one physical replica, with exactly one designated as the leader. Leaders are elected automatically, initially following a first-come-first-served approach and then utilizing a process managed by ZooKeeper for subsequent elections. If a leader replica becomes unavailable, another replica is automatically elected as the new leader. When a document is sent to a Solr node for indexing, the system determines which shard the document belongs to and which node currently hosts the leader for that shard. The document is then forwarded to the current leader for indexing, and the leader distributes the update to all other replicas.

## ZooKeeper Integration
Solr relies on ZooKeeper to maintain its cluster state, including information about which servers host which cores, shards, or parts of the complete collection, as well as configuration files and other essential data that needs to be consistent across the cluster. However, the actual request is made to Solr itself, and Solr internally utilizes information from ZooKeeper to route the request to the appropriate location within the cluster for processing. This ensures efficient query routing and indexing operations in SolrCloud.

