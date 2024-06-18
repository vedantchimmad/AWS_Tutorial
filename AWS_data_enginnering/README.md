# AWS Data Engineering 

---
### 1) Difference between EBS and AWS S3

| statement     | AWS EBS                                    | AWS S3                                                                            |
|---------------|--------------------------------------------|-----------------------------------------------------------------------------------|
| Stands        | Elastic block store                        | SImple storage service                                                            |
| Storage       | It is block level network storage          | It is an object storage service                                                   |
| Use cases     | It is used as disk to attach EC2 instance  | used to store and retrieve large amount of object, static website, backup         |
| Access        | Accessed by EC2 b which volume is attached | Data storage in S3 can be accessed by http and https protcal by unique object url |
| pricing model | charging based on provisioned capacity     | stored data size, number of requests                                              |

### 2) What is IAM in AWS
IAM is Identity Access Management global services provided by AWS to securely control access to the aws resource 

### 3)Describe a VPC and its components
VPC(virtual private cloud) is a virtual private logically isolated network used to lunch AWS resource in private network 
- **Subnets**: A range of IP addresses in your VPC.
- **Route Tables**: They determine where network traffic is directed
- **Internet Gateways**: Connects a network to the internet.
- **NAT Gateways**: Allow private subnets to connect to the internet or other AWS services but prevent the internet from initiating a connection with those instances.
- **Network ACLs**: A layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets.

### 4) How do you secure data at rest
* **Encryption**: Using AWS services such as Amazon S3, EBS, and RDS which support
  encryption at rest,AWS offers integrated tools like AWS KMS (Key Management
  Service) and AWS CloudHSM to manage and rotate encryption keys
* **Access Control**: Implementing fine-grained access control using IAM roles and policies
  to ensure only authorized users and systems can access your data.
* **Network Security**: Utilizing VPCs and associated security features such as security
  groups and network ACLs to isolate resources and control network access

### 5)What are some strategies to reduce costs in AWS?
- Reserved Instances: Purchase Reserved Instances for services like Amazon EC2 and
  Amazon RDS to save up to 75% compared to on-demand pricing.
- Auto Scaling: Use Auto Scaling to automatically adjust the amount of computational
  resources based on the server load, thus reducing costs by minimizing idle resources.
- Right Sizing: Regularly review and adjust your configurations to ensure you are using
  the optimal resources for your workloads.
- Delete Unattached EBS Volumes: Regularly delete unattached EBS volumes, as you are
  billed for the storage that you provision.
- Use Cost Explorer: AWS Cost Explorer helps you visualize and manage your AWS
  spending over time.

### 6) What are S3 Buckets?
An S3 bucket is a container for storing objects in Amazon S3. 

### 7) What are some of the storage classes available in S3?
S3 offers several storage classes:
- S3 Standard: For frequently accessed data, offers high durability, availability, and
  performance.
- S3 Intelligent-Tiering: Moves data automatically between two access tiers when
  access patterns change.
- S3 Standard-IA: For data that is less frequently accessed but requires rapid access
  when needed.
- S3 One Zone-IA: Similar to Standard-IA but data is stored in a single availability zone.
- S3 Glacier and S3 Glacier Deep Archive: For archiving data with retrieval times ranging
  from minutes to hours.
### 8) How does S3 Encryption work?
- Server-Side Encryption (SSE): Where Amazon handles the encryption process, the
  decryption, and key management. You can choose among three keys management
  options: 
   * SSE-S3 (uses keys managed by S3), 
   * SSE-KMS (uses AWS Key Management Service), 
   * SSE-C (where you manage the encryption keys).
- Client-Side Encryption: Your data is encrypted on the client side before uploading it to
  S3.
### 9) Explain the difference between a security group and a network access control list
In the context of AWS, a security group and a network access control list (ACL) are both used to control the traffic flow in and out of your virtual private cloud (VPC), but they operate at different networking layers and have distinct functionalities.

| Security Group                                                                                                                    | Network Access Control List (NACL)                                                                                                                    |
|-----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| Operates at the instance level                                                                                                    | Operates at the subnet level within a VPC                                                                                                             |
| Acts as a virtual firewall for one or more instances.                                                                             | Acts as a firewall for controlling traffic entering or leaving one or more subnets.                                                                   |
| Controls traffic based on security group rules that specify allowed inbound and outbound traffic                                  | ontrols traffic based on numbered ACL entries, each specifying whether to allow or deny specific types of traffic from specific sources/destinations. |
| Stateful: If you allow inbound traffic, responses to that traffic are automatically allowed back out regardless of outbound rules | Stateless: You must define both inbound and outbound rules separately;                                                                                |
| Supports allow rules only                                                                                                         | Supports both allow and deny rules                                                                                                                    |

### 10)What is an Internet Gateway, and why is it used
An Internet Gateway (IGW) is a VPC component that allows communication between instances in your VPC and the internet
