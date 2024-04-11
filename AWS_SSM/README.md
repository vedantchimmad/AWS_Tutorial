# AWS SSM

---
### SSM Parameter Store
![AWS SSM](../Image/AWS_SSM.png)
* Secure storage for configuration and secrets
* Optional Seamless Encryption using KMS
* Serverless, scalable, durable, easy SDK
* Version tracking of configurations / secrets
* Security through IAM
* Notifications with Amazon EventBridge
* Integration with CloudFormation
### SSM Parameter Store Hierarchy
* /my-department/
  * my-app/
    * dev/
      * db-url
      * db-password
    * prod/
      * db-url
      * db-password
    * other-app/
* /other-department/
* /aws/reference/secretsmanager/secret_ID_in_Secrets_Manager
* /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2 (public)