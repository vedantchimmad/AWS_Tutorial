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