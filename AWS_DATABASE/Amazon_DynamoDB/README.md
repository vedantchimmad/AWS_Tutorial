# DynamoDB

---
### Overview
* Non-relational Key-Value store
* Fully Managed, Serverless, NoSQL database in the cloud
* Fast, Flexible, Cost-effective, Fault Tolerant, Secure
* Multi-region, multi-master database (Global Tables)
* Backup and restore with PITR (Point-in-time Recovery)
* Single-digit millisecond performance at any scale
* In-memory caching with DAX (DynamoDB Accelerator, microsecond latency)
* Supports CRUD (Create/Read/Update/Delete) operations through APIs
* Supports transactions across multiple tables (ACID support)
* No direct analytical queries (No joins)
* Access patterns must be known ahead of time for efficient design and performance
### Terminology Compared to SQL
| SQL / RDBMS                             | DynamoDB                                                         |
|-----------------------------------------|------------------------------------------------------------------|
| Tables                                  | Tables                                                           |
| Rows                                    | Items                                                            |
| Columns                                 | Attributes                                                       |
| Primary Keys – Multicolumn and optional | Primary Keys – Mandatory, minimum one and maximum two attributes |
| Indexes                                 | Local Secondary Indexes                                          |
| Views                                   | Global Secondary Indexes                                         |
### DynamoDB Tables
![DynamoDB Key](../Image/DynamoDB_Key.png)
* Tables are top-level entities
* No strict inter-table relationships (Independent Entities)
* You control performance at the table level
* Table items stored as JSON (DynamoDB-specific JSON)
* Primary keys are mandatory, rest of the schema is flexible
* Primary Key can be simple or composite
* Simple Key has a single attribute (=partition key or hash key)
* Composite Key has two attributes(=partition/hash key + sort/range key)
* Non-key attributes (including secondary key attributes) are optional
### Data Types in DynamoDB
1. Scalar Types
   * Exactly one value
   * e.g. string, number, binary, boolean, and null
   * Keys or index attributes only support string, number and binary scalar types
2. Set Types
   * Multiple scalar values
   * e.g. string set, number set and binary set
3. Document Types
   * Complex structure with nested attributes
   * e.g. list and map
### DynamoDB Consistency
* **Read Consistency**: strong consistency, eventual consistency, and transactional
* **Write Consistency**: standard and transactional
1. Strong Consistency
   * The most up-to-date data
   * Must be requested explicitly
   * If we read just after a write, we will get the correct data
2. Eventual Consistency
   * May or may not reflect the latest copy of data
   * Default consistency for all operations
   * 50% cheaper than strong consistency
   * If we read just after a write, it’s possible we’ll get unexpected response because of replication
3. Transactional Reads and Writes
   * For ACID support across one or more tables within a single AWS account and region
   * 2x the cost of strongly consistent reads
   * 2x the cost of standard writes
### DynamoDB Pricing Model
#### Provisioned Capacity
* You pay for the capacity you provision (= number of reads and writes per second)
* You can use auto-scaling to adjust the provisioned capacity
* Uses Capacity Units: Read Capacity Units (RCUs) and Write Capacity Units (WCUs)
* Consumption beyond provisioned capacity may result in throttling
* Use Reserved Capacity for discounts over 1 or 3-year term contracts (you’re charged a onetime fee + an houtly fee per 100 RCUs and WCUs)
* Uses Capacity Units
  * 1 capacity unit = 1 request/sec
* RCUs (Read Capacity Units)
  * In blocks of 4KB, last block always rounded up
  * 1 strongly consistent table read/sec = 1 RCU
  * 2 eventually consistent table reads/sec = 1 RCU
  * 1 transactional read/sec = 2 RCUs
* WCUs (Write Capacity Units)
  * In blocks of 1KB, last block always rounded up
  * 1 table write/sec = 1 WCU
  * 1 transactional write/sec = 2 WCUs
#### On-Demand Capacity
* You pay per request (= number of read and write requests your application makes)
* No need to provision capacity units
* DynamoDB instantly accommodates your workloads as they ramp up or down
* Uses Request Units: Read Request Units and Write Request Units
* Cannot use reserved capacity with On- Demand mode
* storage, backup, replication, streams, caching, data transfer out charged additionally
* Uses Request Units
  * Same as Capacity Units for calculation purposes
* Read Request Units
  * In blocks of 4KB, last block always rounded up
  * 1 strongly consistent table read request = 1 RRU
  * 2 eventually consistent table read request = 1 RRU
  * 1 transactional read request = 2 RRUs
* Write Request Units
  * In blocks of 1KB, last block always rounded up
  * 1 table write request = 1 WRU
  * 1 transactional write request = 2 WRUs

| Provisioned Capacity Mode                                                                    | On-Demand Capacity Mode                                                               |
|----------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| Typically used in production environment                                                     | Typically used in dev/test environments or for small applications                     |
| Use this when you have predictable traffic                                                   | Use this when you have variable,unpredictable traffic                                 |
| Consider using reserved capacity if you have steady and predictable traffic for cost savings | Instantly accommodates up to 2x the previous peak traffic on a table                  |
| Can result in throttling when consumption shoots up (use auto-scaling)                       | Throttling can occur if you exceed 2x the previous peak within 30 minutes             |
| Tends to be cost-effective as compared to the on-demand capacity mode                        | Recommended to space traffic growth over at least 30 mins before driving more than 2x |
### Calculation
1. **Example 1:** Calculating Capacity Units

**Calculate capacity units to read and write a 15KB item**
* RCUs with strong consistency:
  * 15KB/4KB = 3.75 => rounded up => 4 RCUs
* RCUs with eventual consistency:
  * (1/2) x 4 RCUs = 2 RCUs
* RCUs for transactional read:
  * 2 x 4 RCUs = 8 RCUs
* WCUs:
  * 15KB/1KB = 15 WCUs
* WCUs for transactional write:
  * 2 x 15 WCUs = 30 WCUs

2. **Example 2:** Calculating Capacity Units

**Calculate capacity units to read and write a 1.5KB item**
  * RCUs with strong consistency:
    * 1.5KB/4KB = 0.375 => rounded up => 1 RCU
  * RCUs with eventual consistency:
    * (1/2) x 1 RCUs = 0.5 RCU => rounded up = 1 RCU
  * RCUs for transactional read:
    * 2 x 1 RCU = 2 RCUs
  * WCUs:
    * 1.5KB/1KB = 1.5 => rounded up => 2 WCUs
  * WCUs for transactional write:
    * 2 x 2 WCUs = 4 WCUs
3. **Example 3:** Calculating Throughput

**A DynamoDB table has provisioned capacity of 10 RCUs and 10 WCUs. Calculate the throughput that your application can support:**
  * Read throughput with strong consistency = 4KB x 10 = 40KB/sec
  * Read throughput (eventual) = 2 (40KB/sec) = 80KB/sec
  * Transactional read throughput = (1/2) x (40KB/sec)= 20KB/sec
  * Write throughput = 1KB x 10 = 10KB/sec
  * Transactional write throughput = (1/2) x (10KB/sec)= 5KB/sec