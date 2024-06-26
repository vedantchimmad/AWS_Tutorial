# AWS KMS

---
* Anytime you hear “encryption” for an AWS service, it’s most likely KMS
* AWS manages encryption keys for us
* Fully integrated with IAM for authorization
* Easy way to control access to your data
* Able to audit KMS Key usage using CloudTrail
* Seamlessly integrated into most AWS services (EBS, S3, RDS, SSM…)
* Never ever store your secrets in plaintext, especially in your code!
  * KMS Key Encryption also available through API calls (SDK, CLI)
  * Encrypted secrets can be stored in the code / environment variables
### KMS Keys Types
* KMS Keys is the new name of KMS Customer Master Key
* **Symmetric (AES-256 keys)**
  * Single encryption key that is used to Encrypt and Decrypt
  * AWS services that are integrated with KMS use Symmetric CMKs
  * You never get access to the KMS Key unencrypted (must call KMS API to use)
* **Asymmetric (RSA & ECC key pairs)**
  * Public (Encrypt) and Private Key (Decrypt) pair
  * Used for Encrypt/Decrypt, or Sign/Verify operations
  * The public key is downloadable, but you can’t access the Private Key unencrypted
  * Use case: encryption outside of AWS by users who can’t call the KMS API
### AWS KMS (Key Management Service)
* Types of KMS Keys:
  * AWS Owned Keys (free): SSE-S3, SSE-SQS, SSE-DDB (default key)
  * AWS Managed Key: free (aws/service-name, example: aws/rds or aws/ebs)
  * Customer managed keys created in KMS: $1 / month
  * Customer managed keys imported (must be symmetric key): $1 / month
  * + pay for API call to KMS ($0.03 / 10000 calls)
* Automatic Key rotation:
  * AWS-managed KMS Key: automatic every 1 year
  * Customer-managed KMS Key: (must be enabled) automatic every 1 year
  * Imported KMS Key: only manual rotation possible using alias
### Copying Snapshots across regions
![Copying Snapshots across regions](../Image/Copying_Snapshots_across_regions.png)
### KMS Key Policies
* Control access to KMS keys, “similar” to S3 bucket policies
* Difference: you cannot control access without them
* Default KMS Key Policy:
  * Created if you don’t provide a specific KMS Key Policy
  * Complete access to the key to the root user = entire AWS account
* Custom KMS Key Policy:
  * Define users, roles that can access the KMS key
  * Define who can administer the key
  * Useful for cross-account access of your KMS key
### Copying Snapshots across accounts
![Copying Snapshots across accounts](../Image/Copying_Snapshots_across_accounts.png)
1. Create a Snapshot, encrypted with your own KMS Key (Customer Managed Key)
2. Attach a KMS Key Policy to authorize cross-account access
3. Share the encrypted snapshot
4. (in target) Create a copy of the Snapshot, encrypt it with a CMK in your account
5. Create a volume from the snapshot
### KMS Multi-Region Keys
![KMS Multi-Region Keys](../Image/KMS_Multi_Region_keys.png)
* Identical KMS keys in different AWS Regions that can be used interchangeably
* Multi-Region keys have the same key ID, key material, automatic rotation…
* Encrypt in one Region and decrypt in other Regions
* No need to re-encrypt or making cross-Region API calls
* KMS Multi-Region are NOT global (Primary + Replicas)
* Each Multi-Region key is managed independently
* Use cases: global client-side encryption, encryption on Global DynamoDB, Global Aurora
### DynamoDB Global Tables and KMS Multi-Region Keys Client-Side encryption
![DyanamoDB KMS Global Tables](../Image/KMS_DynamoDB_Gloabal.png)
* We can encrypt specific attributes client-side in our DynamoDB table using the Amazon DynamoDB Encryption Client
* Combined with Global Tables, the client-side encrypted data is replicated to other regions
* If we use a multi-region key, replicated in the same region as the DynamoDB Global table, then clients in these regions can use lowlatency API calls to KMS in their region to decrypt the data client-side
* Using client-side encryption we can protect specific fields and guarantee only decryption if the client has access to an API key
### Global Aurora and KMS Multi-Region Keys Client-Side encryption
![Aurora Global KMS](../Image/Aurora_Gloabal_KMS.png)
* We can encrypt specific attributes client-side in our Aurora table using the AWS Encryption SDK
* Combined with Aurora Global Tables, the client-side encrypted data is replicated to other regions
* If we use a multi-region key, replicated in the same region as the Global Aurora DB, then clients in these regions can use low-latency API calls to KMS in their region to decrypt the data client-side
* Using client-side encryption we can protect specific fields and guarantee only decryption if the client has access to an API key, we can protect specific fields even from database admins
### S3 Replication Encryption Considerations
* Unencrypted objects and objects encrypted with SSE-S3 are replicated by default
* Objects encrypted with SSE-C (customer provided key) can be replicated
* For objects encrypted with SSE-KMS, you need to enable the option
  * Specify which KMS Key to encrypt the objects within the target bucket
  * Adapt the KMS Key Policy for the target key
  * An IAM Role with kms:Decrypt for the source KMS Key and kms:Encrypt for the target KMS Key
  * You might get KMS throttling errors, in which case you can ask for a Service Quotas increase
* You can use multi-region AWS KMS Keys, but they are currently treated as independent keys by Amazon S3 (the object will still be decrypted and then encrypted)
### AMI Sharing Process Encrypted via KMS
![AMI Sharing process](../Image/AMI_Sharing_Process.png)
1. AMI in Source Account is encrypted with KMS Key from Source Account
2. Must modify the image attribute to add a Launch Permission which corresponds to the specified target AWS account
3. Must share the KMS Keys used to encrypted the snapshot the AMI references with the target account / IAM Role
4. The IAM Role/User in the target account must have the permissions to DescribeKey, ReEncrypted, CreateGrant, Decrypt
5. When launching an EC2 instance from the AMI, optionally the target account can specify a new KMS key in its own account to re-encrypt the volumes