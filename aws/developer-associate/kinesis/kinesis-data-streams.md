# DataStreams
## ProducedRecord
- Partition key
- Data Blob (up to 1MB)
- ProductionRate: 1MB/sec or 1000 msg/sec per shard

## ConsumeRecord
- PartitionKey
- SequenceNo.
- DataBlob 
- ConsumptionRate: 
  - 2MB/sec (shared) per shard all consumers
    or
  - 2MB/sec (enhanced) per shard per consumers


- Retention b/w 1 to **365days**
- Ability to reprocess (replay) data
- Immutable data
- Data that share same partition goes to same shard (ordering)

# CapacityModes
## ProvisionedMode
- Choose the number of shards provisioned, scale manually or using API
- Each shard gets 1MB/s in (or 1000 records per second)
- Each shard gets 2MB/s out (classic or enhanced fan-out consumer)
- pay per shard provisioned per hour

## On-demandMode
- No need to provision or manage capacity
- Default capacity provisioned (4MB/s in or 4000 records per hour)
- Autoscale based on observed throughput peal during last 30 days& data in/out per GB
- Pay per stream per hour 

# Encryption
- Control using IAm policies
- Encryption in flight using HTTPS endpoints
- Encryption at rest using KMS
- encryption/decryption on client side
- vpc endpoints are available for kinesis to access within VPC
- Monitor api calls using CloudTail  


# producers
SequenceNumber (unique per partition within shard)
PartitionKey
DataBlob (1 MB)
PutRecord API

# Consumers
## Shared (Classic) Fan-out Consumer - pull
- Low number of consuming applications
- Read throughput: 2MB/sec per shard across all consumers
- Max 5 GetRecords API call/sec
- Latency ~200ms
- Minimize Cost($)
- Poll Data using GetRecords API call
- Returns data for upto 10mb (then throttle for 5 seconds) or upto 10000 records

## Enhanced Fan-out Consumer - push
- Multiple consuming applications for low stream
- Read throughput: 2MB/sec per consumer per shard
- Latency ~70ms
- higher Cost($$$)
- Pushes data to consumers over HTTP/2 (Subscribe to shard API)
- Returns data for upto 10mb (then throttle for 5 seconds) or upto 10000 records
- Soft limit of 5 consumer applications (KCL) per data stream (default)

# kinesis Consumers - AWS Lambda
## Batch
- GetBatch()
- Batch size & batch window
- can process up to 10 batches per shard simultaneously

# KCL
- Each shard is to be read by only one KCl instance
- Progress is checkpointed ninto DynamoDB (needs IAM Access) 