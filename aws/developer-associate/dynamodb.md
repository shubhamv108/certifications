# PutItem
new item or fully replace item

# UpdateItem
Adds or updates an existing item attribute
AtomicCounters

# UpdateExpression
"SET Price = price - :discount"

# ConditionalWrites (for write operations)
- **attribute_exists**: attribute_exists(Price)
- **attribute_not_exists**
- **attribute_type**
- **contains** (for string)
- **begins_with** (for string)
- ProductCategory **IN** (:cat1, :cat2) **and** Price **between** :low and :high
- size (string length)

# GetItem
ProjectExpression to retrieve only certain attributes

# Query
## KeyCondition Expression
- PartitionKey value (must be = operator) required
- Sort Key value (=, >, >=, <, <=, Between, begins with) - optional

## Filter Expression (for read queries)
- Additional filtering after query opn
- Use only with non-key attributes (does not allow HASH or RANGE attributes)

## Returns
The number of specified **Limit**.
Up to 1MB data
else do pagination
Can query a table, a local secondary index or global secondary index

# Scan
Scan entire table & then filter out data.
Consumes a lot of RCU.
Limit impact using **Limit** 
For faster performance use, **Parallel Scan**

# DeleteItem
Ability to perform conditional delete.

# BatchWriteItem
upto 25 put or delete item in one go
upto 16mb of data written or up to 400KB data per item
Can't update item
**UnprocessedItems** for failed write opns (exponential backoff or add WCU)

# BatchGetItem
Returns item from one or more table
up to 100 items, up to 16Mb of data
items are retrieved in parallel tto minimize latency
**UnprocessedKeys** for failed read opns (exponential backoff or add RCU)

# PartiQL
SQl compatible
select, insert, update, delete
query multiple table
run from
1. AWS management console
2. NoSQL workbench for dynamodb
3. dynamodb apis
4. aws cli
5. aws sdk


# LSI
**Alternative Sort Key** (same Partition Key)
Sort Key consists of scalar attribute (String, Number, or Binary)
**Up to 5** LSI per table
Must be defined **at table creation time**
Attribute Projections - can contain some pr all the attributes of the table (KEYS_ONLY, INCLUDE, ALL)

# GSI
Alternate Primary Key (HASH or HASH+RANGE) from the base table
Speed up queries on non-key attributes
The index key consists of scalar attributes (String, Number, or Binary)
Attribute Projections - some pr all the attributes of the table (KEYS_ONLY, INCLUDE, ALL)
Must provision RCUs & WCUs for the index
Can be added or modified after table is created.
```
INDEX GSI (query by "Game_ID")
```

# Indexes & Throttling
## GSI
- If writes are throttled on GSI, then the main table will be throttled.
- Even if the WCU on the main tables are fine
- Choose GSI partition ley carefully, assign WCU capacity carefully.

## LSI
- They use WCUs & RCUs of main table.
- No special throttling considerations

# OptimisticLocking
Each item has an attribute that acts as a _version number_

# DAX
Fully-managed, highly available, seamless in-memory cache for DynamoDB.
Microseconds latency for cached reads & queries.
Doesn't require application logic modification (compatible with existing DynamoDB APIs)
Solves the "Hot Keys" problem (too many reads)
TTl 5 minutes (default)
Up to 10 nodes in cluster
Multi-AZ (3 nodes min recommended for production)
Secure (Encryption at rest with KS, VPC, IAM, CloudTrail, ... )

# Streams
Ordered stream of item-level modifications (create/update/delete) in a table
Stream records can be:
    Sent to kinesis Data Streams
    Read By AWS Lambda
    Read by kinesis Client library applications
Data retention up to 24 hours
Use Cases:
    - react to changes in-real time (welcome email to users)
    - Analytics
    - Insert into derivative tables
    - insert into OpenSearch Service
    - implement cross-origin replication
Ability to choose info written to stream
    1. KEYS_ONLY: only the key attributes of the modified item
    2. NEW_IMAGE: the entire item, as it appears after it was modified
    3. OLD_IMAGE: the entire item, as it appears before it was modified
    4. NEW_AND_OLD_IMAGE

Are made up shards just like kinesis data streams, which arre provisioned by AWS.
Stream needs to be enabled

# TTL
Need to give TTL attribute name after enabling
Auto deleted item after an expiry timestamp
Doesn't consume any WCUs
The TTL attribute must be a "Number" data type with "Unix Epoch Timestamp" value
Expired items deleted within 48 hours of expiration
Expired items needs to be filtered
Expired items are deleted both from LSI & GSI
AS delete opn for each expired item enters the dynamodb streams
Use case: reduce stored data by keeping only current items, adhere to regulatory obligations, ...

# Transactions (ACID)
**Read Modes:** Eventually, Strong, Transactional
**WriteModes:** Standard, Transactional
Consumes **2x** WCUs & RCUs
Operations
    - **TransactGetItems**: one or more getItem operations
    - **TransactWriteItems**: one or more PutItem, UpdateItem and DeleteItem operations
- UseCases: financial transaction, multiplayer game

# Session State Cache
vs ElasticCache
    - In memory, vs serverless
    - both are key/value stores
vs EFS
    - EFS must be attached to EC2 as n/w drive
vs EBS & Instance Store
    - EBS & Instance store can only be used for local caching, not shared caching
vs S#, high latency, not meant for small objects

# Write Sharding
- Candidate_ID (PK), Voter_timestamp (Sort Key), Voter_id
    - keep partition key as Candidate_ID + random Suffix for even distribution
- Two methods:
  1. Sharding using Random Suffix
  2. Sharding using Calculated Suffix

# Write types
1. Concurrent 
2. Conditional
3. Atomic
4. Batch

 
# Copying a table
1. Using AWS Data pipeline
2. Backup and restore int a new table
3. Scan + PutItem + BatchWriteItems

# Security
- VPC endpoints available to access without using the Internet
- Access fully controlled by IAM
- Encryption at rest using AWS KMS and in transit using SSl/TLS
- backup & Restore
  - Point-in-time Recovery (PITR) like RDS
  - No performance impact

# Global Tables
  - Multi-region, multi-active, fully replicated, high performance
  - Availability, **durability**, **fault tolerance**
  - closer to users
  - Disaster recovery (RTO, RPO)
  - Uptime SLA -> 99.999%
  - Easy migration from Region A to Region B
  - Must use on demand-capacity or auto-scaled provisioned capacity
  - Writes to local region & replicated to all replicas (Active-Active).
  - Propagated to other replicas within one second
  - Eventually Consistent across all tables
  - Last writer wins reconciliation
  - Conditional expressions, atomic updates, and transactions a re  ona per region basis
  - rWCU or rRCU
  - Relies on DynamoDB streams for replication

# DynamoDB Local

# Migration
- AWS Database Migration Service (AWS DMS) can be used to migrate to DynamoDb from MongoDB, oracle, MySQL, S3, ...)