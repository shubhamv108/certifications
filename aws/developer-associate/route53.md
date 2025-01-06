# DNS
Domain Name Registrar: Amazon Route 53, GoDaddy
Domain Records: A, AAAA, CNAME, NS,...
Zone File: contains DNS records
Name server: resolves DNS queries (Authoritative or Non-authoritative)
Top Level Domain (TLD): .com,.gov,.org
Second Level Domain (SLD): amazon.com, google.com, ...
Non-root domain: demo.amazon.com, ...

http://api.


# Route53
- highly available, scalable, fully manages and Authoritative DNS
- Domain Registrar
- ability to check health of resources
- 100% availability SLA (only aws service)

## DNS Record
- Domain/subdomain name - eg example.com
- RecordType - eg. A or AAAA
- Value - eg. 12.34.56.78
- Routing Policy - how route53 responds to queries
- TTL - amount of time record cached at dns resolvers

- A - maps to hostname to IPv4
- AAAA - maps hostname to IPv6
- CNAME - maps hostname to another hostname
- NS - Nameserver of hosted zone
	- Controles how traffic is routed for a domain

## Hosted Zones
- Container of records and define how to route traffic to domain and subdomain
- $.50 per month per hosted zone

### Public Hosted Zones 
- contains records that define how to route traffic on the internet

### Private Hosted Zones
- contains records that specify how you route traffic whithin one or more VPC's


#### CNAME
only map to Non toot domain
set ttl

#### Alais 
- both root and non-root domain
- cannot set ttl, set automatically by route 53
- Targets
	- ELB
	- CloudFront Distributions
	- API Gateway
	- Elastic Benastalks
	- S3 Websites
	- VPC Interface endpoints
	- Global Accelerator accelerator
	- Route 53 record in same hosted zone
- Cannot set an alias for an EC2 DNS name