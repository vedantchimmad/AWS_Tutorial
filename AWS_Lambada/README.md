# AWS Lambada

---
### Overview
* Serverless is a new paradigm in which the developers don’t have to manage servers anymore.
* Initially... Serverless == FaaS (Function as a Service)
* It is usually very cheap to run AWS Lambda so it’s very popular
### Why Lambada
* Virtual Servers in the Cloud
* Limited by RAM and CPU
* Continuously running
* Scaling means intervention to add / remove servers
* Virtual functions – no servers to manage!
* Limited by time - short executions
* Run on-demand
* Scaling is automated!
### Benefits
* Easy Pricing:
  * Pay per request and compute time
  * Free tier of 1,000,000 AWS Lambda requests and 400,000 GBs of compute time
* Integrated with the whole AWS suite of services
* Integrated with many programming languages
* Easy monitoring through AWS CloudWatch
* Easy to get more resources per functions (up to 10GB of RAM!)
* Increasing RAM will also improve CPU and network!
### Free tier and Billing
   * Pay per duration: (in increment of 1 ms)
   * 400,000 GB-seconds of compute time per month for FREE
   * == 400,000 seconds if function is 1GB RAM
   * == 3,200,000 seconds if function is 128 MB RAM
   * After that $1.00 for 600,000 GB-seconds
### Serverless in AWS
* AWS Lambda
* DynamoDB
* AWS Cognito
* AWS API Gateway
* Amazon S3
* AWS SNS & SQS
* AWS Kinesis Data Firehose
* Aurora Serverless
* Step Functions
* Fargate
### Language support
* Node.js (JavaScript)
* Python
* Java (Java 8 compatible)
* C# (.NET Core)
* Golang
* C# / Powershell 
* Ruby
* Custom Runtime API (community supported, example Rust)
### Lambada integration

### Synchronous invocation
* Synchronous: CLI, SDK, API Gateway, Application Load Balancer
* Results is returned right away
* Error handling must happen client side (retries, exponential backoff, etc…)
### User Invoked:
* Elastic Load Balancing (Application Load Balancer)
* Amazon API Gateway
* Amazon CloudFront (Lambda@Edge)
* Amazon S3 Batch
### Service Invoked:
* Amazon Cognito
* AWS Step Functions
### Other Services:
•Amazon Lex
•Amazon Alexa
•Amazon Kinesis Data Firehose
### Lambada with ALB
* To expose a Lambda function as an HTTP(S) endpoint…
* You can use the Application Load Balancer(or an API Gateway)
* The Lambda function must be registered in a target group


















### ALB and Lambada permission





Asynchronous Invocations
Services
•Amazon Simple Storage Service (S3)
•Amazon Simple Notification Service (SNS)
•Amazon CloudWatch Events / Event Bridge
•AWS Code Commit(CodeCommitTrigger: new branch, new tag, new push)
•AWS CodePipeline (invoke a Lambda function during the pipeline, Lambda must callback)-----other ----
•Amazon CloudWatch Logs (log processing)
•Amazon Simple Email Service
•AWS CloudFormation
•AWS Config
•AWS IoT
•AWS IoT Events
S3, SNS, CloudWatch Events…
The events are placed in an Event Queue
Lambda attempts to retry on errors
• 3 tries total
• 1 minute wait after 1st , then 2 minutes wait
Make sure the processing is idempotent(in case of retries)
If the function is retried, you will see duplicate logs entries in CloudWatch Logs
Can define a DLQ (dead-letter queue) –SNS or SQS–for failed processing (need correct IAM permissions)
Asynchronous invocations allow you to speed up the processing if you don’t need to wait for the result (ex: you need 1000 files processed)
S3 Event notification
S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication…
Object name filtering possible (*.jpg)
Use case: generate thumbnails of images uploaded to S3
S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer
If two writes are made to a single non-versioned object at the same time, it is possible that only a single event notification will be sent
If you want to ensure that an event notification is sent for every successful write, you can enable versioning on your bucket.

Event source mapping
Kinesis Data Streams
SQS & SQS FIFO queue
DynamoDB Streams
Common denominator: records need to be polled from the source
Your Lambda function is invoked synchronously



Lambda Event Mapper Scaling
Kinesis Data Streams & DynamoDB Streams:
•One Lambda invocation per stream shard
• If you use parallelization, up to 10 batches processed per shard simultaneously
SQS Standard:
•Lambda adds 60 more instances per minute to scale up
•Up to 1000 batches of messages processed simultaneously
SQS FIFO:
•Messages with the same Group ID will be processed in order
•The Lambda function scales to the number of active message groups
Lambada event and context object
Event Object
JSON-formatted document contains data for the function to process
Contains information from the invoking service (e.g., Event Bridge, custom, …)
Lambda runtime converts the event to an object (e.g., dict type in Python)
Example: input arguments, invoking service arguments, …
Context Object
Provides methods and properties that provide information about the invocation, function, and runtime environment
Passed to your function by Lambda at runtime
Example: aws_request_id, function_name, memory_limit_in_mb, …















Lambada destinations
Nov 2019: Can configure to send result to a destination
Asynchronous invocations -can define destinations for successful and failed event:
• Amazon SQS
• Amazon SNS
• AWS Lambda
• Amazon EventBridgebus
Note: AWS recommends you use destinations instead of DLQ now (but both can be used at the same time)
Event Source mapping: for discarded event batches
• Amazon SQS
• Amazon SNS
Note: you can send events to a DLQ directly from SQS
Execution role
Grants the Lambda function permissions to AWS services / resources
Sample managed policies for Lambda:
•AWSLambdaBasicExecutionRole–Upload logs to CloudWatch.
•AWSLambdaKinesisExecutionRole–Read from Kinesis
•AWSLambdaDynamoDBExecutionRole–Read from DynamoDB Streams •AWSLambdaSQSQueueExecutionRole–Read from SQS
•AWSLambdaVPCAccessExecutionRole–Deploy Lambda function in VPC •AWSXRayDaemonWriteAccess–Upload trace data to X-Ray.
When you use anevent source mappingto invoke your function, Lambda uses the execution role to read event data.
Best practice: create one Lambda Execution Role per function
Resource based policies
Useresource-based policiesto give other accounts and AWS services permission to use your Lambda resources
Similar to S3 bucket policies for S3 bucket
An IAM principal can access Lambda:
oif the IAM policy attached to the principal authorizes it (e.g. user access)
oOR if the resource-based policy authorizes (e.g. service access)
When an AWS service like Amazon S3 calls your Lambda function, the resource-based policy gives it access.
Lambada environmental variable
Environment variable = key / value pair in “String” form
Adjust the function behavior without updating code
The environment variables are available to your code
Lambda Service adds its own system environment variables as well
Helpful to store secrets (encrypted by KMS)
Secrets can be encrypted by the Lambda service key, or your own CMK
Logging Monitor
CloudWatch Logs:
•AWS Lambda execution logs are stored in AWS CloudWatch Logs
•Make sure your AWS Lambda function has an execution role with an IAM policy that authorizes writes to CloudWatch Logs
CloudWatch Metrics:
•AWS Lambda metrics are displayed in AWS CloudWatch Metrics
• Invocations, Durations, Concurrent Executions
•Error count, Success Rates, Throttles
•Async Delivery Failures
• Iterator Age (Kinesis & DynamoDB Streams)
Tracing with X-Ray
Enable in Lambda configuration (Active Tracing)
Runs the X-Ray daemon for you
Use AWS X-Ray SDK in Code
Ensure Lambda Function has a correct IAM Execution Role
•The managed policy is called AWSXRayDaemonWriteAccess
Environment variables to communicate with X-Ray
•_X_AMZN_TRACE_ID: contains the tracing header
•AWS_XRAY_CONTEXT_MISSING: by default, LOG_ERROR
•AWS_XRAY_DAEMON_ADDRESS: the X-Ray Daemon IP_ADDRESS:PORT
Customization At The Edge
Many modern applications execute some form of the logic at the edge
Edge Function:
•A code that you write and attach to CloudFront distributions
•Runs close to your users to minimize latency
CloudFront provides two types: CloudFront Functions & Lambda@Edge
You don’t have to manage any servers, deployed globally
Use case: customize the CDN content
Pay only for what you use
Fully serverless
Cloud Front functions and lambada edge use cases
Website Security and Privacy
Dynamic Web Application at the Edge
Search Engine Optimization (SEO)
Intelligently Route Across Origins and Data Centers
Bot Mitigation at the Edge
Real-time Image Transformation
A/B Testing
User Authentication and Authorization
User Prioritization
User Tracking and Analytics
Cloud front functions
Lightweight functions written in JavaScript
For high-scale, latency-sensitive CDN customizations
Sub-ms startup times, millions of requests/second
Used to change Viewer requests and responses:
•Viewer Request: after CloudFront receives a request from a viewer
•Viewer Response: before CloudFront forwards the response to the viewer
Native feature of CloudFront (manage code entirely within CloudFront)
Lambada edge
Lambda functions written in NodeJS or Python
Scales to 1000s of requests/second
Used to change CloudFront requests and responses:
•Viewer Request –after CloudFront receives a request from a viewer
•Origin Request –before CloudFront forwards the request to the origin
•Origin Response –after CloudFront receives the response from the origin
•Viewer Response –before CloudFront forwards the response to the viewer
Author your functions in one AWS Region (us-east-1), then CloudFront replicates to its locations
Lambada by default
By default, your Lambda function is launched outside your own VPC (in an AWS-owned VPC)
Therefore it cannot access resources in your VPC (RDS, ElastiCache, internal ELB…)
Lambda in VPC
•You must define the VPC ID, the Subnets and the Security Groups
•Lambda will create an ENI (Elastic Network Interface) in your subnets
•AWSLambdaVPCAccessExecutionRol e
•Internet access
oA Lambda function in your VPC does not have internet access
oDeploying a Lambda function in a public subnet does not give it internet access or a public IP
oDeploying a Lambda function in a private subnet gives it internet access if you have a NAT Gateway / Instance
oYou can use VPC endpoints to privately access AWS services without a NAT

Lambada function configuration
RAM:
• From 128MB to 10GB in 1MB increments
•The more RAM you add, the more vCPU credits you get
•At 1,792 MB, a function has the equivalent of one full vCPU
•After 1,792 MB, you get more than one CPU, and need to use multi-threading in your code to benefit from it (up to 6 vCPU)
If your application is CPU-bound (computation heavy), increase RAM
Timeout: default 3 seconds, maximum is 900 seconds (15 minutes)
Execution context
The execution context is a temporary runtime environment that initializes any external dependencies of your lambda code
Great for database connections, HTTP clients, SDK clients…
The execution context is maintained for some time in anticipation of another Lambda function invocation
The next function invocation can “re-use” the context to execution time and save time in initializing connections objects
The execution context includes the /tmp directory
Lambda Functions /tmpspace
If your Lambda function needs to download a big file to work…
If your Lambda function needs disk space to perform operations…
You can use the /tmp directory
Max size is 10GB
The directory content remains when the execution context is frozen, providing transient cache that can be used for multiple invocations (helpful to checkpoint your work)
For permanent persistence of object (non temporary), use S3
To encrypt content on /tmp, you must generate KMS Data Keys
Lambada Layers
Custom Runtimes
•Ex: C++ https://github.com/awslabs/aws-lambda-cpp
•Ex: Rust https://github.com/awslabs/aws-lambda-rust-runtime
Externalize Dependencies to re-use them:
File system mounting
Lambda functions can access EFS file systems if they are running in a VPC
Configure Lambda to mount EFS file systems to local directory during initialization
Must leverage EFS Access Points
Limitations: watch out for the EFS connection limits (one function instance = one connection) and connection burst limits



Storage














Lambada concurrency and throttling
Concurrency limit: up to 1000 concurrent executions
Can set a “reserved concurrency” at the function level (=limit)
Each invocation over the concurrency limit will trigger a “Throttle”
Throttle behavior:
• If synchronous invocation => return ThrottleError-429
• If asynchronous invocation => retry automatically and then go to DLQ
If you need a higher limit, open a support ticket
Lambada cloud formation
Inline functions are very simple
Use the Code.ZipFile property
You cannot include function dependencies with inline functions
Through S3
You must store the Lambda zip in S3
You must refer the S3 zip location in the CloudFormation code
•S3Bucket
•S3Key: full path to zip
•S3ObjectVersion: if versioned bucket
If you update the code in S3, but don’t update S3Bucket, S3Key or S3ObjectVersion, CloudFormation won’t update your function
Lambada versions
When you work on a Lambda function, we work on $LATEST
When we’re ready to publish a Lambda function, we create a version
Versions are immutable
Versions have increasing version numbers
Versions get their own ARN (Amazon Resource Name)
Version = code + configuration (nothing can be changed -immutable)
Each version of the lambda function can be accessed
Lambada alias
Aliases are ”pointers” to Lambda function versions
We can define a “dev”, ”test”, “prod” aliases and have them point at different lambda versions
 Aliases are mutable
Aliases enable Canary deployment by assigning weights to lambda functions
Aliases enable stable configuration of our event triggers / destinations
Aliases have their own ARNs
Aliases cannot reference aliases
Function URL
Dedicated HTTP(S) endpoint for your Lambda function
A unique URL endpoint is generated for you (never changes)
https://<url-id>.lambda-url.<region>.on.aws(dual-stack IPv4 & IPv6)
Invoke via a web browser, curl, Postman, or any HTTP client
Access your function URL through the public Internet only
Doesn’t support PrivateLink(Lambda functions do support)
Supports Resource-based Policies & CORS configurations
Can be applied to any function alias or to $LATEST (can’t be applied to other function versions)
Create and configure using AWS Console or AWS API
Throttle your function by using Reserved Concurrency
Auth Type None : allow public and unauthenticated access
•Resource-based Policy is always in effect (must grant public access)







Auth Type IAM: IAM is used to authenticate and authorize requests
Both Principal’s Identity-based Policy & Resource-based Policy are evaluated
•Principal must have lambda:InvokeFunctionUrlpermissions
•Same account –Identity-based Policy ORResource-based Policy as ALLOW
•Cross account –Identity-based Policy ANDResource Based Policy as ALLOW


Limits per region
Execution:
•Memory allocation: 128 MB –10GB (1 MB increments)
•Maximum execution time: 900 seconds (15 minutes)
•Environment variables (4 KB)
•Disk capacity in the “function container” (in /tmp): 512 MB to 10GB
•Concurrency executions: 1000 (can be increased)
Deployment:
•Lambda function deployment size (compressed .zip): 50 MB
•Size of uncompressed deployment (code + dependencies): 250 MB
•Can use the /tmpdirectory to load other files at startup
•Size of environment variables: 4 KB
Lambda best practice
Perform heavy-duty work outside of your function handler
•Connect to databases outside of your function handler
• Initialize the AWS SDK outside of your function handler
• Pull in dependencies or datasets outside of your function handler
Use environment variables for:
•Database Connection Strings, S3 bucket, etc… don’t put these values in your code
• Passwords, sensitive values… they can be encrypted using KMS
Minimize your deployment package size to its runtime necessities.
• Break down the function if need be
• Remember the AWS Lambda limits
•Use Layers where necessary
Avoid using recursive code, never have a Lambda function call itself