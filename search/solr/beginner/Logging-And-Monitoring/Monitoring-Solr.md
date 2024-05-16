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
