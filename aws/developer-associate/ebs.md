- N/w drive that can attch to instances while they run
- persist data even after instance is terminated
- bound to AZ
- snapshot to move volume across AZ||Region
- one ot one attche (instance to EBS)
- "multi-attach" feature for some EBS
- need not to be attched with instance
- latency when communicating to instance over n/w
- detach from ec2 instance and attch to another one quickly, for failover
- provision capacity in advance (GBs & IOPS)
- increase capcity over time

- Delete on termination has to be enbaled


"N/w usb stick"


#### Snapshots
	- Recommended to detach volume from instance befpre snapshot
	- can copy snapshot accross AZ or Region
	- Archive (75% cheaper), Takes 24 to 72 hrs to restore
	- Snapshots go to recycle bin, recover from accidental deletion
	- recycle bin retension (1 day to 1 year)

##### Fasr Snapshot Restore (FSR)
	- Force full initilization of snapshot to no latency on the first use ($$$)


### Types
	- g2/gp3 - SSD
	- io1/io2 (SSD) - for high throughput
	- st1 (HDD) - low cost HDD, frequently accessed, throughput intensive workloads
	- sci (HDD) - Lowest cost HDD, less frequently accessed workloads
	
EBS Volume is defined by Size|Throughput| IOPS
Only g2/g3 and io1/io2 can be used as boot volumes

#### g2/gp3 (SSD)
	- Cost effective, low-latency
	- system boot vol, virtual desktops, dev & test env
	- 1GB to 16TB
	- gp3:
		- baseline of 3000 ops & throughput of 125MB/s
		- can inc iops to 16000 and throughput to 1000 mB/s independently
	- gp2:
		- small vol can burst upto 3000iops
		- size of vol and iops are linked, max iops is 16000
		- 3 iops/GB, means at 5334GB we are at the max IOPS

#### provisioned IOPS (SSD)
	- Critical bussines app with sustained IOPS
	- app that need more than 16000 iops
	- great for databses workloads (sensitive to storage perf and consistency)
	- io1/io2: (4GB - 16TB)
		- Max PIOPS: 64000 for Nitro EC2 instances & 32000 for other
		- can inc piops independently from storage size
		- io2 have more dyrability and more iops per GB (at same price of io1)
	- io2 block express (4Gb - 16 TB)
		- sub millisecond latency
		- max piops: 256000 with an iops:GB ratio of 1000:1
	- Supports EBS multi attach

#### st1 (HDD)
	- cannot be a boot volume
	- 125gb to 16tb
	- throughput optimized HDD (st1)
		- big data, data warehouse, log processing
		- max throughput of 500mb/sec - max iops 500
#### sc1 (Cold HDD)
	- infrequently accessed and archived data
	- max throughput 250MB/sec - max iops 250



## Multi-Attch - io1/io2 family
	- bound to AZ
	- upto 16 instances at a time
	- Attach same EBS vol to multiple EC2 instances in same AZ
	- Each instance has full redad & write permission to high performace vol
	- use cases
		- Achieve higher app availability in clustered linux app
		- app must manage concurrent werite operation
	- use a cluster aware file system (not XFS, EXT4 etc)



