# Creating Collection Using Custom Configset

In this guide, we'll walk through the process of creating a Solr collection using a custom configuration set (configset). We'll cover both the command-line method for uploading a configset to ZooKeeper and the API call to create a collection with the uploaded configset.

## Prerequisites

Before proceeding, ensure that you have the following:

- A custom configset prepared. This configset should include the necessary configuration files (`solrconfig.xml`, `schema.xml`, etc.) for your Solr collection.

## Uploading Configset to ZooKeeper

To upload a custom configset to ZooKeeper, you can use the `bin/solr zk upconfig` command. Here's an example command:

```bash
bin/solr zk upconfig -n <Name of configset> -d /path/to/configset_directory/ -z localhost:9983
```

> When configuring Solr, note that Zookeeper and Solr run on different ports. For an external Zookeeper installation, use the default port 2181. If Zookeeper is embedded in Solr (in our case), the port is Solr's port plus 1000, typically 9983. Ensure to use the appropriate port based on your Zookeeper setup.

Replace /path/to/configset_directory/ with the path to your custom configset directory.

- `-n <Name of configset>`: Specifies the name of the configset.
- `-d /path/to/configset_directory/`: Specifies the path to the directory containing the configset files. Replace with the path to your custom configset directory.
- `-z localhost:9983`: Specifies the ZooKeeper connection string.

For example

```bash
bin/solr zk upconfig -n instanceDetails -d /home/shannee/temp/solr-8.11.3/server/solr/configsets/instanceDetails/conf/ -z localhost:9983
```

## Creating Collection Using Custom Configset API
Once the configset is uploaded to ZooKeeper, you can create a Solr collection using the custom configset by making an API call to Solr. Here's an example API call:


```bash
http://localhost:8983/solr/admin/collections?action=CREATE&name=myNewCollection&numShards=1&replicationFactor=1&collection.configName=instanceDetails
```

Replace myNewCollection with the desired name for your Solr collection.

Parameters breakdown:

- `action=CREATE`: Specifies the action to create a collection.
- `name=myNewCollection`: Specifies the name of the collection to be created.
- `numShards=1`: Specifies the number of shards for the collection.
- `replicationFactor=1`: Specifies the replication factor for the collection.
- `collection.configName=instanceDetails`: Specifies the name of the configset to be used for the collection.

By following the steps outlined in this guide, you can easily create a Solr collection using a custom configset.

### Task 
Add the following fields to instanceDetails collection.

- instanceUrl: String type, Single value, Indexed, Required, Stored
- status: String type, Single value, Indexed, Not required, Stored
- builtOn: Date type, Single value, Indexed, Not required, Stored
- version: String type, Single value, Indexed, Not required, Stored
- branch: String type, Single value, Indexed, Not required, Stored
- revision: String type, Single value, Indexed, Not required, Stored
- plugins* (dynamic field): String type, Single value, Indexed, Not required, Stored
- systemInfo* (dynamicField): String type, Single value, Indexed, Not required, Stored
- searchServer* (dynamicField): String type, Single value, Indexed, Not required, Stored
