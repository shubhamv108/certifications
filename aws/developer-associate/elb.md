## Type
### Classic Load balancer
	- HTTP/2, HTTPS, TCP, SSl (secure TCP)
### ALB
	- HTTP, HTTPS, WebSocket
	- Layer7
	- balances HTTP app across target group
	- KB on multiple app on same EC2 instance
	- support redirect (from HTTp to HTTPS)
	- Routing table to diff target groups
		- Route based on path in URL
		- Route based on hostnam ein URL
		- Route based on Query string, Header
	- great for micro-services & container based app
	- Port mapping to redirct to a dynamic port in ECS 
	- Target Groups
		- EC2 instances (can be managed by ASG) - HTTP
		- ECS tasks (managed by ECS itself) - HTTP
		- Lambda functions - HTTP request is transalated into a json event
		- IP adresses - must be private

		- ALB can route to multiple target groups
		- Health check done at target group level
	- Fixed hostname
	- app servers do not see ip of client directly
	- X-Forwarded-For = ip of client
	- X-Forwareded-Port = port
	- X-Forwareded-Proto = protocol used

### NLB
	- TCP, TLS (secure TCP), UDP
	- Layer 4
	- handle millions of req/sec
	- ~100ms (vs 400 ms for ALB)
	- Has one IP per AZ, support assigning ElasticIP
	- used for extreme performance, TCp or UDP traffic
	- not included in AWS free tier
	- Target Groups
		- EC2 instances
		- IP Addresses - must be private IPs
		- ALB (IP on NLB, HTTP rule on ALB)
	- Healt checks support TCPm HTTP, HTTPS


### Gateway Load Balancer
	- Layer 3
	- Deploy, scale, manage a fleet of 3rd Party N/W virtual appliances in AWS
	- Firewalls Intrusion Detection & Prvention, Deep Packet Inspection, payload manipulation
	- Has 2 fns
		1. Transparent N/W Gateway: single entry/exit for all traffic
		2. Load Balancer: distributes the traffic to your virtual appliances
	- Uses GENEVE protocol on port 6081
	- Target Groupsfor CLB
		- EC2 instances
		- IP Addresses - must be private IPs


## Sticky Sessions
	- CLB, ALB, NLB
	- Cookie used for stickiness has an expiration date you control (NLB works without cookie)
	- user doesn't lose his session data
	- Stcikiness brings imbalance to the load over the backend EC2 instances
	- Cookie
		1. Application-based Cookies
			a. Custom Cookie
				- generated by the target
				- can include any custom attribute reqd by app
				- cookie name must be specified individually for each target group
				- don't use AWSALB, AWSALBAPP, AWSALBTG (reserved for use by the ELB)
			b. Application Cookie
				- Generated by LB
				- Cookie name is AWSALBAPP
		2. Duration-based Cookies
			- Cookie generated by the LB
			- Cookie name is AWSALB for ALB, AWSELB for CLB

## Cross-Zone LB
	- ALB
		- Enabled by default (can be disabled at Target Group level)
		- No charges for inter AZ data
	- NLB, GLB
		- Disbaled by default
		- You pay $ for inter AZ data if enabled
	- CLB
		- Disbaled by default
		- No charges for inter AZ data transfer

## SSLCertificate
	- Manage certificate using ACM
	- create and upload certificates
	- HTTPS listener:
		- must specify a default certificate
		- add an optional list of certs to support multiple domains
		- Clinets can use SNI(Server Name Indication) to specify the hostname they reach
		- specify a security policy to support older versions of SSL/TLS (legacy clients)
	- CLB
		- Support only SSl certificate
		- Must use multiple CLB for multiple hostname with multiple SSL certificates
	- ALB
		- Supports multiple listeners with multiple SSL certificates
		- Uses NI to make it work
	- NLB
		- Supports multiple listeners with multiple SSL certificates
		- Uses NI to make it work
#### ServeNameIndication (SNI)
	- Solves the problem of loading multiple SSL certificates onto one web server (to serve multiple websites)
	- it is a newer protocol that requires the client to indicate the hostname of the target server in thie initial SSL handshake
	- server will then find the correct certificate or return default one
	- Note: On;y works for ALB, NLB, Cloudfront; 
	- Does not work on CLB


## ConnectionDraining
	- Naming
		- Connection Draining - CLB
		- Deregister Delay - ALB, NLB
	- Time to complete "in-flight requests" while the instance is de-registered or unhealthy
	- Stops sending new reqts to EC2 instance which is de-registering
	- b/w 1 to 3600 secs (default: 300 secs)
	- can be disable (set value to 0)
	- set to low value if your requests are short


## AutoScalingGroup
	- Goals
		- Scale out (add EC2 instances) to match an increased load or vice verse
		- Ensure min/max ec2 instances running
		- Auto register new instnaces to a load balncer
		- Re-create an EC2 instance in case previos one is terminated
	- FREE
	- Capacities - Initial/Min/Desired/Max
	- ELB health check can be passed to ASG
	- Attributes
		- A Launch Template 
			- AMI _ Instance Type
			- EC2 user Dara
			- IEBS Volumes
			- Security Groups
			- SSh Key pair
			- IAM Roles for your EC2 Instances
			- N/W + Subnet info
			- LB Info
		- Initial/Min/Desired/Max capacity
		- Scaling policies

#### ASG - CloudWatch Alarms & Scaling
	- It is possible to scale an ASG based on cloudwatch alarms
	- Alarm metrics (such as Average CPU, or a custom metric) 
##### ASG - SclingPolicies
	- Target Tracking Scalinf
		- Average ASG CPU to stay aroung 40%
	- Simple/Step Scaling
		- CloudWatch Alarm (CPU > 70%), then add 2 units
		- CloudWatch Alarm (CPU < 30%), then remove 1 units
	- Scheduled Actions
		- Anticipate a scling based on known usage patterns
		- Ex: inc min capacity to 10 at 5pm on Fridays
	- Predictive Scaling
		- continuosly forecast load &  schedule scalng ahead
###### Metrics for sclaing
	1. Average CPU
	2. RequestCountPerTarget
	3. Average N/W In?Out (if you are app is n/w bound)
	4. Any custom metric (thout you push using Cloudwatch)
###### Scaling Cooldown
	- After scaling activity happens you are in cooldown period (default 300 secs)
	- During cooldown period ASG will not launch or terminate additional instances (to allow for metrics to stabilize)
	- use ready to use AMI to reduce config time in order be serving request fasters and reduce the cooldown preiod
