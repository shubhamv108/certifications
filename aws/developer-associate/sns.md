- -pub/sub
- one message to many receivers 
- filter messages per subscriber
- upto 12,500,000 subscribers
- 100,000 topics limit

# Encryption
- in-flight: HTTPS API
- at-rest:
    - Amazon SQS key (**SSE-SQS**)
    - KMS keys
- client-side encryption

# Access Control
- IAM policies to regulate access to SQS API

# SNS polices (similar to s3 bucket policies )
- cross account access to sns topic
- useful for other services to write to a sns queue

