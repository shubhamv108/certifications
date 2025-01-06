- push once in sns, receive all in sqs queue that are subscribers
- fully decoupled no data lodd
- sqs allows for: data persistence, delayed processing & retries for work
- ability to more sqs subscribers over time
- make sure your sqs queue access policy allows for SNS to write
- Cross region delivery

# s3 events to multiple queues

# SNS -> KinesisDataFirehose -> Amazon S3


# FIFO Topic
- ordering of messages in the topic

- ordering by message group id
- deduplicaiton using deduplicationid

both sqs standard 7 FIFO queue can be subscribers

# For Fanout + ordering + deduplication

SNS(FIFO topic) -> SQS(FIFO Queue)