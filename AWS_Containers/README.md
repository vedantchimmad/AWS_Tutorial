# AWS Docker

---
* Docker is a software development platform to deploy apps
* Apps are packaged in containers that can be run on any OS
* Apps run the same, regardless of where they’re run
  * Any machine
  * No compatibility issues
  * Predictable behavior
  * Less work
  * Easier to maintain and deploy
  * Works with any language, any OS, any technology
* Use cases: microservices architecture, lift-and-shift apps from onpremises to the AWS cloud, …
### Docker Containers Management on AWS
* Amazon Elastic Container Service (Amazon ECS)
  * Amazon’s own container platform
* Amazon Elastic Kubernetes Service (Amazon EKS)
  * Amazon’s managed Kubernetes (open source)
* AWS Fargate
  * Amazon’s own Serverless container platform
  * Works with ECS and with EKS
* Amazon ECR:
  * Store container images
## Amazon ECS
### Amazon ECS - EC2 Launch Type
![ECS Ec2](../Image/AWS_ECS_EC2.png)
* ECS = Elastic Container Service
* Launch Docker containers on AWS = Launch ECS Tasks on ECS Clusters
* EC2 Launch Type: you must provision & maintain the infrastructure (the EC2 instances)
* Each EC2 Instance must run the ECS Agent to register in the ECS Cluster
* AWS takes care of starting / stopping containers
### Amazon ECS – Fargate Launch Type
![Fargate](../Image/AWS_ECS_Fargate.png)
* Launch Docker containers on AWS
* You do not provision the infrastructure(no EC2 instances to manage)
* It’s all Serverless!
* You just create task definitions
* AWS just runs ECS Tasks for you based on the CPU / RAM you need
* To scale, just increase the number of tasks. Simple - no more EC2 instances
### Amazon ECS – IAM Roles for ECS
![ECS IAM](../Image/ECS_IAM.png)
* EC2 Instance Profile (EC2 Launch Type only):
  * Used by the ECS agent
  * Makes API calls to ECS service
  * Send container logs to CloudWatch Logs
  * Pull Docker image from ECR
  * Reference sensitive data in Secrets Manager or SSM Parameter Store
* ECS Task Role:
  * Allows each task to have a specific role
  * Use different roles for the different ECS Services you run
  * Task Role is defined in the task definition
### Amazon ECS – Load Balancer Integrations
![ECS Load balancer](../Image/ECS_Loadbalancer.png)
* Application Load Balancer supported and works for most use cases
* Network Load Balancer recommended only for high throughput / high performance use cases, or to pair it with AWS Private Link
* Classic Load Balancer supported but not recommended (no advanced features – no Fargate)
### Amazon ECS – Data Volumes (EFS)
![ECS Volumes](../Image/ECS_data%20Volumes.png)
* Mount EFS file systems onto ECS tasks
* Works for both EC2 and Fargate launch types
* Tasks running in any AZ will share the same data in the EFS file system
* Fargate + EFS = Serverless
* Use cases: persistent multi-AZ shared storage for your containers
* Note:
  * Amazon S3 cannot be mounted as a file system
### ECS Service Auto Scaling
* Automatically increase/decrease the desired number of ECS tasks
* Amazon ECS Auto Scaling uses AWS Application Auto Scaling
  * ECS Service Average CPU Utilization
  * ECS Service Average Memory Utilization - Scale on RAM
  * ALB Request Count Per Target – metric coming from the ALB
* **Target Tracking** – scale based on target value for a specific CloudWatch metric
* **Step Scaling** – scale based on a specified CloudWatch Alarm
* **Scheduled Scaling** – scale based on a specified date/time (predictable changes)
* ECS Service Auto Scaling (task level) ≠ EC2 Auto Scaling (EC2 instance level)
* Fargate Auto Scaling is much easier to setup (because Serverless)
### EC2 Launch Type – Auto Scaling EC2 Instances
![ESC Auto scaling](../Image/ECS_Scaling.png)
* Accommodate ECS Service Scaling by adding underlying EC2 Instances
* Auto Scaling Group Scaling
  * Scale your ASG based on CPU Utilization
  * Add EC2 instances over time
* ECS Cluster Capacity Provider
  * Used to automatically provision and scale the infrastructure for your ECS Tasks
  * Capacity Provider paired with an Auto Scaling Group
  * Add EC2 Instances when you’re missing capacity (CPU, RAM…)
### ECS tasks invoked by Event Bridge
![ECS tasks invoked by Event Bridge](../Image/ECS_TaskBy_EventBridge.png)
### ECS tasks invoked by Event Bridge Schedule
![ECS tasks invoked by Event Bridge Schedule](../Image/ECS_Task_EventBridge_Schedule.png)
### ECS – SQS Queue Example
![ECS – SQS Queue Example](../Image/ECS_SQS.png)
### ECS – Intercept Stopped Tasks using EventBridge
![ECS – Intercept Stopped Tasks using EventBridge](../Image/ECS_Intercept_Stopped.png)