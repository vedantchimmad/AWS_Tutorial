# Amazon SQS

---
### Standard Queue
* Oldest offering (over 10 years old)
* Fully managed service, used to decouple applications
* Attributes:
  * Unlimited throughput, unlimited number of messages in queue
  * Default retention of messages: 4 days, maximum of 14 days
  * Low latency (<10 ms on publish and receive)
  * Limitation of 256KB per message sent
* Can have duplicate messages (at least once delivery, occasionally)
* Can have out of order messages (best effort ordering)
### Producing Messages
* Produced to SQS using the SDK (SendMessage API)
* The message is persisted in SQS until a consumer deletes it
* Message retention: default 4 days, up to 14 days
* Example: send an order to be processed
  * Order id
  * Customer id
  * Any attributes you want
* SQS standard: unlimited throughput
### Consuming Messages
![Consuming Messages](../Image/SQS_consuming_messages.png)
* Consumers (running on EC2 instances, servers, or AWS Lambda)…
* Poll SQS for messages (receive up to 10 messages at a time)
* Process the messages (example: insert the message into an RDS database)
* Delete the messages using the DeleteMessage API