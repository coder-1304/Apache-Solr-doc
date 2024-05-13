# SolrCloud and Zookeeper Behavior

Solr utilizes Zookeeper to maintain its cluster state, which includes information about the distribution of cores, shards, and collections across the SolrCloud cluster, as well as configuration files and other cluster-related data. Requests made to Solr are internally routed based on information retrieved from Zookeeper. It means that the request itself is made to Solr, and Solr uses information from Zookeeper in the background to route the request internally to the correct location

## Starting a SolrCloud with 1 Zookeeper node and 3 SolrCloud nodes:

### Case 1: Creating a Collection with Replication Factor 1

If we create a collection with a replication factor of 1 and then index some data, it will create only one core on a SolrCloud instance. To identify which SolrCloud instance hosts that core, we can use the following API:

```
http://172.23.0.4:8983/solr/admin/collections?action=clusterstatus&wt=json&collection=<Collection Name>
```


### Case 2: Data Availability and Failover

Indexed data in a collection with only one core on a SolrCloud instance will be available for searching in all other SolrCloud instances. However, if the SolrCloud instance hosting that core is stopped, the data won't be available.

### Case 3: Updating Replication Factor

If we create a collection with a replication factor of 1 and later try to update the replication factor, it will not automatically create replicas for previously indexed data. Changing the replication factor only updates the Zookeeper z-node, not the topology of the collection. However, it allows more `ADDREPLICA` commands to succeed.

Adding replicas can be achieved using the Solr Collection API. For example:

```
http://localhost:8983/solr/admin/collections?action=ADDREPLICA&collection=shipment&shard=shard1
```


After adding replicas, indexed data will be distributed across the new replicas, ensuring high availability and fault tolerance.
