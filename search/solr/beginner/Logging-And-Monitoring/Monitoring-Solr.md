This file includes details of Solr’s metrics<sup>[9]</sup> registries and Metrics API.

Solr provides tools for developers to track and measure how well it's performing. This helps them understand what's happening under the hood and how to optimize it for better results.
To do this, Solr uses a system called Dropwizard Metrics, which has different tools for measuring different aspects of performance:

- **Counters**: These simply keep track of how many times something happens, like the number of requests Solr receives.
- **Meters**: These not only count events but also calculate how often they happen over time, like how many requests Solr gets per minute.
- **Histograms**: These give a more detailed view by showing the distribution of events, like the average, median, and maximum request times.
- **Timers**: These measure both the number of events and how long they take, providing insights into performance bottlenecks.
- **Gauges**: These give a snapshot of a specific value at a given moment, like how many active connections Solr has right now.

Sometimes, some of these measurements might be missing, but Solr handles this gracefully by returning null values so that missing data doesn't skew the overall picture.

Solr organizes these metrics into groups and manages them using something called a metric registry. Each group corresponds to a different aspect of Solr, like its Java Virtual Machine (JVM), web server (Jetty), or individual cores.

Additionally, Solr can send these metrics to external systems for further analysis. Currently, it supports sending them to systems like JMX, Ganglia, Graphite, and SLF4J for monitoring and troubleshooting.

Lastly, Solr provides a special endpoint called /admin/metrics that developers can query to get detailed reports on these metrics, allowing them to monitor Solr's performance in real-time.


# Metric Registries 

In Solr, metric registries are like organized folders where related metrics are stored. These metrics basically track various aspects of Solr's performance and behavior.

**Key Points:**

- **Continuous Tracking:** Metrics are continuously monitored and accumulated from the moment Solr starts until it shuts down. For instance, if you're looking at metrics for a specific SolrCore (a unit of Solr indexing), they are tracked even during operations like loading, unloading, or renaming the core. These metrics are only cleared when the core is explicitly deleted.

- **No Persistence Across Restarts:** However, it's important to note that these metrics aren't saved across restarts. So, if you restart Solr, all previously collected metrics are wiped out.

## Types of Metric Groups:

1. **JVM Registry:**
   - Includes various metrics related to the Java Virtual Machine (JVM) like memory usage, CPU time, garbage collection (GC) activities, thread counts, etc.
   - Also, it provides information about system properties like Java version, directory paths, ports, etc. You can customize what appears here by tweaking `solr.xml`.

2. **Overseer Registry:**
   - Specifically for Solr running in Cloud mode (SolrCloud).
   - Tracks metrics related to the size of Overseer queues, which manage tasks like collection work and cluster state updates.

3. **Node / CoreContainer Registry:**
   - Contains metrics related to the overall Solr node and its CoreContainer.
   - Tracks handler requests for various tasks like collections, administrative tasks, and configuration settings. Also monitors the number of cores and their status.

4. **Core (SolrCore) Registry:**
   - Focuses on individual cores within Solr, with each core having its own set of metrics.
   - Monitors common request handlers, index-level events like merges and replication, and connection statuses for various handlers.

5. **Jetty Registry:**
   - Deals with metrics related to the Jetty server, which handles HTTP requests in Solr.
   - Tracks thread and connection pools, request/response times, and response rates categorized by HTTP response classes.
