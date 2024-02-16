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

Since first film is named `.45`, Solr will guess the field type as float. We don't want that because the name of the movies are string. So, setting up the "name" field in Solr before we index the data to be sure Solr always interprets it as a string.

URL- http://localhost:8983/solr/films/schema

Method - Post

Content-type: application/json

Request body:

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

In the basic-indexing-example, we didnt' have to specify a field to search because the configuration we used was set up to copy fields into a text field
The configuration we are using doesn't have that rule. We need to define field to search for every query.
However we can set up a "catchall field" by defining a copy field that will take all data from all fields and index it into a field named "_text_".
To do this:
URL- http://localhost:8983/solr/films/schema

Method - Post

Content-type: application/json

Request body:

```json
{
  "add-copy-field": {
    "source": "*",
    "dest": "_text_"
  }
}
```
What it does it make a copy of all fields and put the data into the "_text_" field.

## Indexing sample film data

```bash
bin/post -c films example/films/films.json
```
# Faceting
Allows the search results to be arranged into subsets (or buckets or categories), providing a count for each subset.

**Types**
- Field values
- Numeric and data ranges
- Pivots (decision tree)
- Arbitrary query faceting

## Field Facets
Solr query can also return the number of documents that contain each unique value in the whole result set.
To allow this add parameters:
- `&rows=0`
- `&facet=true`
- `&facet.field=genre_str`

Response:
```json
{
  "responseHeader":{
    "zkConnected":true,
    "status":0,
    "QTime":11,
    "params":{
      "q":"*:*",
      "facet.field":"genre_str",
      "rows":"0",
      "facet":"true"}},
  "response":{"numFound":1100,"start":0,"maxScore":1.0,"docs":[]
  },
  "facet_counts":{
    "facet_queries":{},
    "facet_fields":{
      "genre_str":[
        "Drama",552,
        "Comedy",389,
        "Romance Film",270,
        "Thriller",259,
        "Action Film",196,
        "Crime Fiction",170,
        "World cinema",167]},
        "facet_ranges":{},
        "facet_intervals":{},
        "facet_heatmaps":{}}}
```

## Range Facets
Range facets in Solr allow users to aggregate search results into predefined numerical or date ranges, enabling efficient filtering and analysis based on specific value intervals. They provide insights into the distribution of data across different ranges, aiding in understanding data distribution patterns.

To allow this add parameters:
- `&rows=0`
- `&facet=true`
- `&facet.range=initial_release_date`
- `&facet.range.start=NOW-20YEAR`
- `&facet.range.end=NOW`
- `&facet.range.gap=+1YEAR` (replace '+' with '%2B' for URL encoding)

This will request all films and ask for them to be grouped by year starting with 20 years ago

Response:

```json
{
  "responseHeader":{
    "zkConnected":true,
    "status":0,
    "QTime":8,
    "params":{
      "facet.range":"initial_release_date",
      "facet.limit":"300",
      "q":"*:*",
      "facet.range.gap":"+1YEAR",
      "rows":"0",
      "facet":"on",
      "facet.range.start":"NOW-20YEAR",
      "facet.range.end":"NOW"}},
  "response":{"numFound":1100,"start":0,"maxScore":1.0,"docs":[]
  },
  "facet_counts":{
    "facet_queries":{},
    "facet_fields":{},
    "facet_ranges":{
      "initial_release_date":{
        "counts":[
          "1997-07-28T17:12:06.919Z",0,
          "1998-07-28T17:12:06.919Z",0,
          "1999-07-28T17:12:06.919Z",48,
          "2000-07-28T17:12:06.919Z",82,
          "2001-07-28T17:12:06.919Z",103,
          "2002-07-28T17:12:06.919Z",131,
          "2003-07-28T17:12:06.919Z",137,
          "2004-07-28T17:12:06.919Z",163,
          "2005-07-28T17:12:06.919Z",189,
          "2006-07-28T17:12:06.919Z",92,
          "2007-07-28T17:12:06.919Z",26,
          "2008-07-28T17:12:06.919Z",7,
          "2009-07-28T17:12:06.919Z",3,
          "2010-07-28T17:12:06.919Z",0,
          "2011-07-28T17:12:06.919Z",0,
          "2012-07-28T17:12:06.919Z",1,
          "2013-07-28T17:12:06.919Z",1,
          "2014-07-28T17:12:06.919Z",1,
          "2015-07-28T17:12:06.919Z",0,
          "2016-07-28T17:12:06.919Z",0],
        "gap":"+1YEAR",
        "start":"1997-07-28T17:12:06.919Z",
        "end":"2017-07-28T17:12:06.919Z"}},
    "facet_intervals":{},
    "facet_heatmaps":{}}}
```

## Pivot Facets (decision trees)

Allows 2 or more fields to be nested for all the various possible combinations

Instead of just providing counts for individual values within a single field, pivot facets allow you to group and aggregate values across multiple fields, creating a hierarchical representation of data.

- `&facet=true`
- `&facet.pivot=genre_str, directed_by_str`

Response:

```json
{"responseHeader":{
    "zkConnected":true,
    "status":0,
    "QTime":1147,
    "params":{
      "q":"*:*",
      "facet.pivot":"genre_str,directed_by_str",
      "rows":"0",
      "facet":"on"}},
  "response":{"numFound":1100,"start":0,"maxScore":1.0,"docs":[]
  },
  "facet_counts":{
    "facet_queries":{},
    "facet_fields":{},
    "facet_ranges":{},
    "facet_intervals":{},
    "facet_heatmaps":{},
    "facet_pivot":{
      "genre_str,directed_by_str":[{
          "field":"genre_str",
          "value":"Drama",
          "count":552,
          "pivot":[{
              "field":"directed_by_str",
              "value":"Ridley Scott",
              "count":5},
            {
              "field":"directed_by_str",
              "value":"Steven Soderbergh",
              "count":5},
            {
              "field":"directed_by_str",
              "value":"Michael Winterbottom",
              "count":4}}]}]}}}
```

# Updating data:

- Solr does not duplicate data while uploading multiple times
- Because the example solr schema specifies a unique key field called id
- Whenever you POST commands to solr to add a document with the same value for the unique key as an existing document, it automatically replaces it for you
  
# Deleting data:

```bash
bin/post -c localDocs -d "<delete><id>SP2514N</id></delete>"
```
This will delete the document with id=SP2514N

**To delete all documents**
```bash
  bin/post -c localDocs -d "<delete><query>*:*</query></delete>"
```
