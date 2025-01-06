## ECS-AutoScaling
- Inc/Dec desired number of ECS tasks
- Uses AWS Application Auto Scaling
	- AWS Application Auto Scaling
	- Average CPU Utilization
	- Average Memory utilization
	- ALB Request Count per target

- Kind of AutoScaling
	- Target Tracking
	- Step Scaling
	- Schedule Scaling - (predictable changes)


# ECS Launch Type - Auto Sclaing EC2 Instances
- Auto Scaling Grounp Scaling
- ECS Cluster Capacity Provider (use this)


# Load Balancing
## EC2 Launch Type
- ALB works with Dynamic Host Port Mapping for each tsk in EC2 Instance

## Fargate
- Each task has unique private IPthrough ENI
- ECS ENI Security Group needs to allow 80 from ALB Security Group
- ALB Security Group needs to allow security group from web

# IAM
- One IAM Role per task definition

# Environment Varaibles
- Harcoded
- SSM parameter Store
- Secret manager
- S3

# ECS Data Volumes - Data bind
- Share datab/w multiple containers in same task definition
- Mount data volume into both container
- Works for both EC2 and Fargate tasks
## EC2Tasks 
	- Using EC2 instance storage
	- Data are tied to lifecycle of EC2 Instance
## FagateTasks
	- Using Ephemeral Storage
	- Data are tied to containers using them
	- 20GB to 200GB

## Task Placement Strategy
	1. Binpack 
		- place task based on least available CPU or Memroy
		- min the no. of instances in use
	2. Random
	3. Spread
		- Place the task evenly based on the specified value
		- example: instanceId, attribute.ecs.availabilityzone
## Task Placement Constraint
	1. DistinctInstance
	2. memberOf