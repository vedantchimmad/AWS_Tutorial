# AWS Snow Family

---
* Highly-secure, portable devices to collect and process data at the edge, and migrate data into and out of AWS
* Data migration:Snowcone, Snowball Edge, Snowmobile
* Edge computing:Snowcone,Snowball Edge
### Snowball Edge (for data transfers)
* Physical data transport solution: move TBs or PBs of data in or out of AWS
* Alternative to moving data over the network (and paying network fees)
* Pay per data transfer job
* Provide block storage and Amazon S3-compatible object storage
* Snowball Edge Storage Optimized
* 80 TB of HDD capacity for block volume and S3 compatible object storage
* Snowball Edge Compute Optimized
* 42 TB of HDD or 28TB NVMe capacity for block volume and S3 compatible object storage
* Use cases: large data cloud migrations, DC decommission, disaster recovery
### AWS Snowcone & Snowcone SSD
* Small, portable computing, anywhere, rugged & secure, withstands harsh environments
* Light (4.5 pounds, 2.1 kg)
* Device used for edge computing, storage, and data transfer
* Snowcone – 8 TB of HDD Storage
* Snowcone SSD – 14 TB of SSD Storage
* Use Snowcone where Snowball does not fit (spaceconstrained environment)
* Must provide your own battery / cables
* Can be sent back to AWS offline, or connect it to internet and use AWS DataSync to send data
### AWS Snowmobile
* Transfer exabytes of data (1 EB = 1,000 PB = 1,000,000 TBs)
* Each Snowmobile has 100 PB of capacity (use multiple in parallel)
* High security: temperature controlled, GPS, 24/7 video surveillance
* Better than Snowball if you transfer more than 10 PB
### AWS OpsHub
* Historically, to use Snow Family devices, you needed a CLI (Command Line Interface tool)
* Today, you can use AWS OpsHub (a software you install on your computer / laptop) to manage your Snow Family Device
  * Unlocking and configuring single or clustered devices
  * Transferring files
  * Launching and managing instances running on Snow Family Devices
  * Monitor device metrics (storage capacity, active instances on your device)
  * Launch compatible AWS services on your devices(ex: Amazon EC2 instances, AWS DataSync, Network File System (NFS))
>[!NOTE]
>
> • Snowball cannot import to Glacier directly
>
> • You must use Amazon S3 first, in combination with an S3 lifecycle policy