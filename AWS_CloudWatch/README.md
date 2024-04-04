# AWS CloudWa

---
## Amazon CloudWatch Metrics
* CloudWatch provides metrics for every services in AWS
* Metric is a variable to monitor (CPUUtilization, NetworkIn…)
* Metrics belong to namespaces
* Dimension is an attribute of a metric (instance id, environment, etc…).
* Up to 30 dimensions per metric
* Metrics have timestamps
* Can create CloudWatch dashboards of metrics
* Can create CloudWatch Custom Metrics (for the RAM for example)
### CloudWatch Metric Streams
![CloudWatch Metric streams]()
* Continually stream CloudWatch metrics to a destination of your choice, with near-real-time delivery and low latency.
  * Amazon Kinesis Data Firehose (and then its destinations)
  * 3rd party service provider: Datadog, Dynatrace, New Relic, Splunk, Sumo Logic…
* Option to filter metrics to only stream a subset of them