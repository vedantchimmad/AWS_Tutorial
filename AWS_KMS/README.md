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
* Symmetric (AES-256 keys)
  * Single encryption key that is used to Encrypt and Decrypt
  * AWS services that are integrated with KMS use Symmetric CMKs
  * You never get access to the KMS Key unencrypted (must call KMS API to use)
* Asymmetric (RSA & ECC key pairs)
  * Public (Encrypt) and Private Key (Decrypt) pair
  * Used for Encrypt/Decrypt, or Sign/Verify operations
  * The public key is downloadable, but you can’t access the Private Key unencrypted
  * Use case: encryption outside of AWS by users who can’t call the KMS API