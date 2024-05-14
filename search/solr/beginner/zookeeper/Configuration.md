# ZooKeeper Ensemble Configuration Guide

When deciding on the number of ZooKeeper (ZK) nodes to use in your deployment, it's essential to consider both reliability and performance factors. Here's a guide to determine whether to use a single ZK node or a multi-node ensemble and the difference it will make.

## Single ZooKeeper Node vs. Multi-Node Ensemble

### Reliability:
- **Single Node**: A standalone ZooKeeper server lacks reliability. A single node failure can bring down the entire ZK service, leading to service interruptions.
- **Three-Node Ensemble**: With a 3-node ensemble, a single server failure can be tolerated while still maintaining service availability. This setup leverages simple majority voting, allowing the ensemble to continue operating even if one node fails.
- **Recommended Configuration**: For increased reliability, it's recommended to have at least 3 ZK servers in production environments. A 5-node ensemble provides additional redundancy, enabling you to sustain planned maintenance or unexpected outages without interrupting service.

### Performance:
- **Write Performance**: Adding more ZK servers can decrease write performance due to the need for synchronization across the ensemble. Write operations, such as updating configuration or modifying cluster state, require consensus among all nodes, leading to increased latency with more servers.
- **Read Performance**: While write performance may decrease, read performance experiences modest improvements with additional ZK servers. Read operations primarily access data locally on each node, independent of ensemble size. Therefore, read efficiency remains consistent regardless of the ensemble's scale.

## Recommendations:
- **Starting Configuration**: For many deployments, starting with a 3-node ensemble strikes a balance between fault tolerance, performance, and resource utilization.
- **Production Environments**: In online production environments, consider deploying a 5-node ensemble for increased reliability and redundancy. This configuration allows for planned maintenance and unexpected outages without service interruption.

It's important to note that when determining the ensemble size, prioritize reliability over performance considerations, as ZooKeeper's primary role is to ensure data consistency and coordination in distributed systems.

In summary, for SolrCloud deployments, the ideal number of ZooKeeper nodes ranges from 3 to 5, balancing fault tolerance and resource efficiency. Starting with 3 nodes allows for tolerance of a single failure, while 5-node ensembles offer increased reliability. This setup ensures service continuity during planned maintenance or unexpected outages without interruption. Prioritizing reliability over performance, selecting the optimal ensemble size is crucial for stable SolrCloud deployments.
