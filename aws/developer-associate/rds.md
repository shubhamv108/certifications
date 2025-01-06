- Automated provisioning, OS patching
- Continuos backups & restore to specific timestamp (Point in Time Restore)
- Monitoring dashboards
- Read replicas for improved read performance
- Multi AZ setup for DR (Disaster Recovery)
- Maintanance windows for the upgrades
- Scaling capability (vertical & horizontal)
- Storage is backed by EBS (gp2 or io1)
- Can't SSH into your instances
- MariaDB, MySQL, PostgreSQL, SQL Server, Oracle, DB2, Aurora MySQL, Aurora PostgreSQL

## Storage Auto Scaling
	- Helps you increase storage on your RDS db instance dynamically
	- When RDS detectes you are running out of free db storage, it scales automatically
	- Avoid manual scaling of db storage
	- Maximum Storage Threshold (max limit for db storage)
	- Auto modify storage if:
		- Free storage is less than 10% of allocated storage
		- Low-storage lasts atleast 5 mins
		- 6 hrs have passed since last mod
	- Useful for app with unpredictable load

## ReadReplicas (ASYNC)
	- Upto 15 Read Replicas
	- Within AZ, Cross AZ, Cross Region
	- ASYNC Replication from main to Read Replicas, so reads are eventually consistent
	- Can be promoted to own DB
	- App must update the con string to leverage read replicas

## N/W Cost
	- RDS Read Replica within same region, no fees when data goes from one AZ to another

## MultiAZ (DisasterRecovery) (SYNC)
	- SYNC replication
	- One DNS name - auto app failover to standby
	- Increare availability
	- Failover incasr of loss of AZ, loss of n/w, instance or storage failure
	- No manual intervention in apps
	- Not used for scaling (just for standby, no on can read/write to it)

## Single AZ to Multi AZ
	- Zero downtime opn (no need to stop DB)
	- just click on modify anable Multi AZ
	- It creates snapshot of main db and restore in standby db, then SYNC replication to catchup to main DB

## Availability & Durability
	- Multi-AZ DB Cluster
	- Multi-AZ DB Instance
	- Single DB Instance

# Aurora
	- PostgreSQL, MySQL
	- Cloud Optimized 5x over MySQl on RDS, 3x on PostgreSQL
	- Storage automatically grows auto increementally from 10GB to 128TB
	- upto 15 replicas and replication process is faster than MySQL (sub 10ms replication lag) (ASYNC replication) (SYNC AZ replication)
	- Instantaneous failover (HA native)
	- Cost 20% > RDS, but more efficient
	- 6 copies of data across 3AZ
		- 4 copies out of 6 for writes
		- 3 copis out of 6 for reads
		- Self healing with peer-to-peer replication
		- Storage is striped across 100s of volumes
	- One Arora instance takes write (master)
	- Automated failover for master in less than 30 secs
	- Master + upto 15 read replicas serve reads
	- Writer Endpoint (Master), ReaderEndpoint (Connect to one of the read replicas), LB at connection level
	- Features
		- Automatic fail-over
		- Backup & Recovery
		- Isolation & Security
		- Industry Compliance
		- Push-button scaling
		- Automated Patching with Zero Downtime
		- Advanced Monitoring
		- Route Maintenance
		- Backtrack: restore data at any point in time using backups

## Security
	- Encryption
		- master & read replica encryption using AWS KMS at launch time
		- master is not encrypted, read replica cannot be encrypted
		- to encrypt an unencrypted db, go through a DB snapshot & restore as encrypted
	- In-flight encryption: TLS-ready by default, use AWS TLS root certificates client side
	- IAM Auth: IAM Roles to connect to Db
	- Security Groups: To control N?W access to your RDS
	- No SSH available except on RDS custom
	- Audit Logs can be enabled and sent to CloudWatch Logs for longer retention


# RDSProxy
	- Pool and share DB connections - improve db efficiency by reducing stress on DB resources (eg CPU, RAM) this minimize open con & timeoutes
	- Serveeless, autoscaling, highly available (Multi-AZ)
	- Reduced RDS & Aurora failover time by 66%
	- MySQL, PostgreSQL, MariaDB, MS SQL Server, Aurora (MySQL, PostgreSQL)
	- No code change reqd for most apps
	- Enforce IAM auth for DB, and secure connection in AWS Secrets Manager
	- Never publically accesible (must be accessed from VPC)
	- Specially good for lambdas to pool connections


## Cross Region Read Replica (ASYNC) then MULtiAZ (SYNC)