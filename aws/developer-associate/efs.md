- Managed NFS that can be mounted on many EC2
- ec2 instances can be in Multi-AZ
- highly available, scalable, expensive (3x gp2), pay per use
- uses NFSv4.I protocol
- use security group to control access to EFS
- only compatible with linux based AMI
- encryption using KMS
- POSIX file system, that has standard file API
- scales automatically, pay per use, no capacity planning

### use cases
	- contant mangement
	- web serving
	- data sharing
	- wordpress


### Scale
	- 1000s of concurrent EF clients, 10Gb+ throughput
	- grow to petabyte-scale nfs, automatically

### Performance mode (set EFS creation time)
	- general purpose (default) - latency sensitive use case (web server, CmS etc)
	- max i/o -  higher latency, thoughput, high;y parallel (big data, media processing)

### Throughput Mode
	- Bursting - 1Tb - 50MB/s + burst upto 100MB/s
	- Provisioned - set you throughput regardless of storage size; 1Gbs for 1 Tb storage
	- Elastic - automatically scales throughput up/down based on you workloads
		- up to 3GB/s for read & 1GB for write
		- use for unpredictable workloads


### Storage classes
##### Storage Tiers (lifecycle management - move files after N days)
	- Standard: for frequently accessed files
	- infrequent access (EFS-IA): 
		- cost to retrieve files
		- lower price to store
		- enable efs -iA with a lifecycle  Policy

### Availability & durability
	- Standard: Multi AZ, great for prod
	- One Zone: One AZ, great for dev, backup enbaled by default, compatible with IA 
		(EFS One Zone-IA) 


