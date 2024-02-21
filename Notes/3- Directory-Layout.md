# Solr Directory Documentation

After installing Solr, you'll find the following directories and important files within them:

## `bin/`

This directory contains several essential scripts that simplify using Solr.

- **`solr`** and **`solr.cmd`**: Solr's Control Script, used for starting and stopping Solr, creating collections or cores, configuring authentication, and working with configuration files in SolrCloud mode. Configuration files control how Solr indexes, searches, and serves data. 

- **`post`**: The PostTool, offering a simple command line interface for POSTing content to Solr.

- **`solr.in.sh`** and **`solr.in.cmd`**: These files contain configuration settings for Java, Jetty, and Solr at the system level. While you can adjust many of these settings using the bin/solr or bin/solr.cmd scripts, using these property files allows you to centrally manage all configurations in one location.

- **`install_solr_services.sh`**: This script is used on *nix systems to install Solr as a service. This means configuring Solr to run automatically as a background service upon system startup, ensuring continuous availability without manual intervention.

## `contrib/`

Contains add-on plugins for specialized features of Solr. It contains additional plugins and extensions that provide specialized features to enhance Solr's functionality. These plugins are like extra tools that you can add to Solr to enable specific capabilities or integrate with other systems, making Solr more powerful and versatile for different use cases.

## `dist/`

The "dist" directory in Solr contains the primary Solr ".jar" files. A JAR file allows Java runtimes to efficiently deploy an entire application, including its classes and their associated resources, in a single request. These files are the main Java Archive files that comprise the core components of the Solr application. They include the necessary libraries, classes, and resources that make up the Solr software. In other words, the "dist" directory holds the essential files needed to run Solr, and it serves as the core distribution of the Solr software.

## `docs/`

Includes a link to online Javadocs for Solr.

## `example/`

Includes various examples demonstrating Solr capabilities. Solr comes with various example documents and pre-configured setups that you can use as starting points when beginning to work with it.

## `licenses/`

Contains all licenses for 3rd party libraries used by Solr.

## `server/`


Replace $SOLR_INSTALL with the location where you have extracted the Solr installation bundle.

## Directory Layout

The /server directory is organized as follows:

- **contexts**: Contains the Jetty Web application deployment descriptor for the Solr Web app.
- **etc**: Holds Jetty configuration files and an example SSL keystore.
- **lib**: Contains Jetty and other third-party libraries.
- **logs**: Stores Solr log files.
- **resources**: Contains configuration files, such as log4j2.xml, for configuring Solr loggers. Specifies how Solr should handle logging
- **scripts/cloud-scripts**: Includes command-line utilities for working with ZooKeeper in SolrCloud mode.
- **solr**: Default solr.solr.home directory where Solr creates core directories. Must contain solr.xml. The core directory in Apache Solr contains metadata and configuration files related to the indexed documents. 
- **solr/configsets**: Contains different configuration options for running Solr, including default and sample configurations.

## Solr Examples

Solr provides various example configurations to help you get started quickly:

- **_default**: Minimal configurations with field-guessing and managed schema turned on, allowing you to start indexing data without designing a schema upfront.
- **sample_techproducts_configs**: Comprehensive example configuration demonstrating many powerful features of Solr, suitable for building a search solution for tech products.

This documentation provides an overview of the directories and files in the Solr directory. Each component plays a vital role in configuring and running Solr effectively.
