To Decouple Applications

# Attributes
  - Unlimited throughput, unlimited number of messages in queue
  - Default retention of messages 4 days, max 14 days
  - low Latency (< 10ms on publish & receive)
  - **256kb per message**
- can have duplicate messages (atleast once delivery, occasionally)
- can have out of order messages (Best effort ordering)
- SendMessage API
- POll messages (upto **10 messages at a time**)

- At least once delivery
- best effort message ordering
- Competing Consumers

# Alarms
CloudWatchMetric - QueueLength (increase capacity of autoscaling group)

# Encryption
- in-flight: HTTPS API
- at-rest: 
  - Amazon SQS key (**SSE-SQS**)
  - KMS keys
- client-side encryption

# Access Control
- IAM policies to regulate access to SQS API

# SQS polices (similar to s3 bucket policies )
- cross account access to sqs queues
- useful for other services to write to a sqs queue

# Visibility Timeout
Default: **30 seconds**
Consumer could call ChangeMessageVisibility API 

# DLQ
After the **MaximumReceives** threshold is exceeded, the message goes into DLQ
DLQ for Statndard Queue must also be a StandardQueue
## RedriveToSource

# DelayQueue
- upto 15 mins
- Can override the default on send using the **DelaySeconds** parameter

# LongPolling
- 1 sec to 20 sec (**20 sec preferable**)
- can be set at queue level or at API level **ReceiveMessageWaitTimeSeconds**

# API
- CreateQueue (MessageRetentionPeriod), DeleteQueue
- PurgeQueue
- SendMessage (DelaySeconds), ReceiveMessage, DeleteMessage
- MaxNumberOfmessges: default 1, max 10 (ReceiveMessage API)
- ReceiveMessageWaitTimeSeconds: Long polling
- ChangeMessageVisibility

- Batch APIs help in reduce cost

# FIFO Queue
- ordering of messages in the queue using **GroupId**
- 300 messages/sec without batching, 3000 msgs/sec
- exactly once send capability (by removing duplicates)
- messages are processed in order by the consumer


## **DeDuplication**
1. **ContentBasedDeduplication** - SHA-256 hash of message body
2. **DeDeuplicationId**

## MessageGrouping
- if same value if MessageGroupId in an SWS FIFO queue you can only have 1 consumer, & all messages are in order
- To get ordering at level of a subset of messages, specify diff values for **MessageGroupId**
  - message that share a common MessagegroupId wil be in order within the group
  - each group id can have a diff consumer (parallel processing)
  - ordering across groups is not gauranteed

# SQSExtendedClientLibrary
- For sending messages > 256KB