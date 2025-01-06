Elastic Compute Cloud

#### Capabilities
	1. Renting Virtual Machines (EC2)
	2. Storing Data on Virtual Machines (EBS)
	3. Distributing Load machine (ELB)
	4. Scaling the services using an auto-scaling group (ASG)


#### UserData
	1. bootstrapping means launching commands when machine starts
	2. script iis only run once when machine starts
	3. Automate boot taks such as
		- Installing updates
		- Installing softwares
		- Downloding common files ffrom the internet
		- Any think you can think of

	Note: Longer wait time
	Any command will have sudo rights


#### Instances Purchase
##### On-Demand Instances
- Short workload
- predicatable pricing
- pay by seconds
- linux/windows - billing per sec
- other os per hour
- highest cost but no upfront payment
- no long term commitment
- unpredictable workloads


##### Reserved (1(+discount) & 3 years)
	- 72% discount to on-demand
	- reserve instance attribute (instancetype, region, tenancy , os)
	- Reserved Instances - long workloads
	- convertibale reserved instances - long workloads with flexible instances
	- Savings Plan (1(+) & 3(+++) years) - commit t a amount of usage, workloads
	- paymentOptuoions: no upfront(+), partial upfront(++), all upront(+++)
	- InstanceScopr: Region or AZ
	- steady state usage appliations (like database)

	- Convertibale Reserved Instance
		Allows chaning instance type, family, os, scope, tenancy
		upto 66% discount


#### Saving plan
	- upto 72 % discount
	- commit to a certain type of usage ($10/hr for 1 to 3 years)
	- beyond savings plan at on demad price
	- locked to specific instance family & region
	- flexible (instance size, region, tenancy , os)

##### Spot Instances
	- 90% compared on demand
	- most cost efficient
	- bacthc jobs, 
	- data anlysis
	- image processing
	- any distributed workloads
	- workloads with flexible start & end time
	- Short workloads, cheap, can lose anytime (less reliable)

##### Dedicated Hosts
	- book an entire physical server, control instance placement
	- compliance reqts, existing server bound software licenses
	- purchase options
		- on demad, reserve
	- most expensive
	- bring your own license

##### Dedicated Instances
	- no other customer will share your h/w
	- may share h/w with instances in same account
	- no control over instance placement (can move h.w after start/stop)

##### CapacityReservation
	- reserve capacity in any AZ for specific duration
	- no time commitment, no billing discuount
	- regional reserved instances, savings plan
	- charged at on-demand rate
	- short term uniterrepted workloads in specific az
