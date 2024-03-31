# Amazon Cognito

---
* Give users an identity to interact with our web or mobile application
* Cognito User Pools:
  * Sign in functionality for app users
  * Integrate with API Gateway & Application Load Balancer
* Cognito Identity Pools (Federated Identity):
  * Provide AWS credentials to users so they can access AWS resources directly
  * Integrate with Cognito User Pools as an identity provider
* Cognito vs IAM: “hundreds of users”, ”mobile users”, “authenticate with SAML”