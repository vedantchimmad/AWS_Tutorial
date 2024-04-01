# Amazon EMR
* EMR stands for “Elastic MapReduce”
* EMR helps creating Hadoop clusters (Big Data) to analyze and process vast amount of data
* The clusters can be made of hundreds of EC2 instances
* EMR comes bundled with Apache Spark, HBase, Presto, Flink…
* EMR takes care of all the provisioning and configuration
* Auto-scaling and integrated with Spot instances
* Use cases: data processing, machine learning, web indexing, big data…
### Amazon EMR – Node types & purchasing
* Master Node: Manage the cluster, coordinate, manage health – long running
* Core Node: Run tasks and store data – long running
* Task Node (optional): Just to run tasks – usually Spot
* Purchasing options:
  * On-demand: reliable, predictable, won’t be terminated
  * Reserved (min 1 year): cost savings (EMR will automatically use if available)
  * Spot Instances: cheaper, can be terminated, less reliable
* Can have long-running cluster, or transient (temporary) cluster