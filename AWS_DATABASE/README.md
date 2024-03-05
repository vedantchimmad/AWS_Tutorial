# Database Basics

---
A database is an organized collection of structured information, or data
## Basics of data 
#### DATA
a collection of values or information
* **Structured Data and Schema:**

<img src="../Image/Structured_data.png" width="700"/>

* Structured data is organized information that follows a predefined format.
* A schema defines the structure and organization of data within a database, exemplified by tables in a relational database.
* Suitable for OLTP (Online transactional process)and OLAP (Online analytical Process) workloads
* Suited for complex queries and analytics e.g. complex table join operations
* In Amazon RDS (Relational Database Service), users can define a schema for their databases, specifying tables, columns, and relationships (Amazon RDS - Working with DB Instances).
* **Semi-Structured Data:**

<img src="../Image/Semi_structured_data.png" width="500"/>

* Semi-structured data is organized but doesn't strictly adhere to a schema, allowing for varied attributes. 
* Well suited for big data and low-latency applications Examples: XML and JSON
* Typical of non-relational databases
* AWS DynamoDB: Suited for semi-structured data and scenarios requiring high performance and low latency (Amazon DynamoDB - Data Model).
* **Unstructured Data:**

<img src="../Image/Un_structured_data.png" width="400"/>

* Unorganized data,No defined schema Example: documents, photos, videos, music
* Typical of non-relational databases, file systems, data stores or data lakes like Amazon S3
* Searching and organizing unstructured data can be more challenging compared to structured or semi-structured data.

## Database
### 1. Relational Databases:
* Relational databases organize data into tables with predefined relationships and use SQL (Structured Query Language) for querying.
* Have a predefined schema
* Enforce strict ACID compliance and supports “joins”
* Example: Microsoft SQL, Oracle, MySQL, Postgres, Aurora, and MariaDB.
* Interconnected Tables: Tables in a relational database are linked through relationships, often established using foreign keys pointing to primary keys.
* AWS Services:Amazon RDS and Aurora for OLTP, Redshift for OLAP
##### Structured Query Language (SQL):
* SQL is a language used to interact with relational databases, enabling the retrieval and manipulation of data.
* Relational databases support complex queries, including joins, to retrieve data from multiple related tables.
* Amazon RDS supports SQL-based interactions with various relational database engines (Amazon RDS - Using SQL with a DB Instance).
##### What is ACID compliance
* ACID stands for : Atomicity, Consistency, Isolation, and Durability
* **Atomicity means** : “all or nothing” – a transaction executes completely or not at all
* **Consistency means** : once a transaction has been committed, the data must conform to the given schema
* **Isolation** : requires that concurrent transactions execute separately from one another
* **Durability** : is the ability to recover from an unexpected system failure or outage
* if it were MySQL tables with relationships defined on the DB, no application would be able to override the behavior.
### 2. Non-relational Databases (NoSQL):
* Suitable for semi-structured and unstructured data* NoSQL Meaning: NoSQL stands for "not only SQL."
* Semi-Structured Data: NoSQL databases are suitable for data that doesn't neatly fit into a schema, allowing for more flexibility in attribute definitions.
* Amazon DynamoDB: Ideal for big data applications with high performance and low latency requirements (Amazon DynamoDB Documentation).
* Suited for big data applications
* **Big data** : High Volume, High Velocity, High Variety