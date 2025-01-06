- Support for WebSocket Protocol
- Handle API versioning (v1, v2...)
- Handle different environment
- Handle security (Authentication & Authorization)
- Create API keys, handle request throttling
- Swagger / OpenAPI import to quickly define APIs 
- Transform & validate requests & responses
- generate SDk & API specifications
- Cache API responses

# Integrations
## Lambda Function (default timeout 29 sec)
## HTTP
## AWS Service

# EndpointTypes
## Edge Optimized (default): For global clients
- Requests are routed through CloudFront Edge locations (improves latency)
- The API gateway still **lives only in one region**
## Regional
- For clients within same region
- Could control the CloudFront (more control over the caching strategies & th distribution)
## Private
- Can only be accessed from your VPC using an interface VPC endpoint
- Use a resource policy to define access

# Security
- User Authentication
    - IAM Roles (useful for internal applications)
    - Cognito (identity for external users - e.g. mobile users)
    - Custom Authorizer
- custom Domain Name HTTPS security through AWS Certificate Manager (ACM)
    - If using Edge-optimized endpoint, then the certificate must be in us-east-1
    - If using Regional endpoint, then the certificate must be in API Gateway region.
    - Must set up CNAME or A-alias record in Route53

# DeploymentStages
- Changes are deployed to "Stages"
- Each stage has it's own configuration parameters
- Stages can be rolled back as history of deployment can be kept
## StageVariables
- Are like environment variables for API Gateway
- Use them to chnage often changing configuration values
- They can be used in
  - Lambda function ARN
  - HTTP Endpoint
  - Parameter mapping templates
- Use cases
  - Configure HTTP endpoints your stages talk to (dev, test, prod...)
  - Pass config parameters to Lamda through mapping templates
- Stage variables are passed to context object in AWS lambda
- Format: **${stageVariables.variableName}**
- divide 95%, 5% to differrent versions of lambda functions

# CanaryDeployments

# IntegrationType
- MOCK
  - API gateway returns response without sending request ot the backend
- HTTP / AWS
  - integrate both request ans response
  - setup data template mapping using mapping templates for the request & response
- AWS_PROXY (Lambda Proxy)
- HTTP_PROXY
  - Add HTTP header (ex: API Key)

# MappingTemplates
- To modify requests/ responses
- Rename / Modigy query parameters
- Modify body content
- Add headers
- uses Velocity Template Language (VTL): for loop, if etc...
- Filter unnecessary data
- Content-Type application/json or application/xml


# Open API spec
- Import existing OpenAPI spec to API Gateway
  - Method
  - Method Request
  - Integration Request
  - Method Response
  - + AWS extensions for API gateway & setup every single option
- Export currnt API as OpenAPI spec
- can be written in JSON or XML
- using OpenAPi we can generate SDK for out applications
## RequestValidation
- Checks
  - The required request parameters in the URI, query string, and headers of an incoming request are included & non-blank
  - The applicable request payload adheres to the configured JSON Schema request model of the method
### OpenAPI Definitions file
```
{
  "openapi": "3.0.0",
  "info": {
    "title": "",
    "version": ""
  },
  "servers": [...],
  "x-amazon-apigateway-request-validators": {
    "all": {
      "validateRequestBody": true,
      "validateRequestParameters": true
    },
    "params-only": {
      "validateRequestBody": false,
      "validateRequestParameters": true
    }
  },
  "paths": {
    "/validation": {
      "post": {
        "x-amazon-apigateway-request-validators": "all"
      }
    }
  }
}
```


# Caching
- TTL: 300seconds (min; 0s, max: 3600s)
- Are defined per stage
- Possible tooverride cache settings per method
- Encryption option
- Size: 0.5GB to 237Gb
- Expensive avoid in dev/test
## Invalidation
- From UI immediately
- Clients can invalidate with header: **Cache-Control: max-age=0** (with proper IAM authorization)
- If no InvalidateCache policy, any client can invalidate the API cache

# UsagePlans & API Keys
- Header: **x-api-key**

# Logging & tracing
- At Stage level (with log level: ERROR, DEBUG, INFO)
- override settings per API basis

# Metrics
- By Stage
- **CacheHitCount** & **CacheMissCount**
- **Count**
- **IntegrationLatency**
- **Latency**
- **4XXError** & **5XXError**

# Throttling
- Account
  - 10000 rps across all apis
  - Soft limit can be inc upon request
  - return 429
  - Can set S**tage limit** & **Method Limit**
  - **UsagePlan** per customer

# Error
400: badreques
403: AccessDenied, WAF filtered
429

502: Bad Gateway
503: Service Unavailable
504: Integration Failure
  ex endpoint request timed-out exception
  API gateway requests timeout after 29 seconds max

# CORS
- CORS must be enabled to receive API calls from another domain
- OPTIONS pre-flight request must contain the following headers
  - **Access-Control-Allow-Methods**
  - **Access-Control-Allow-Headers**
  - **Access-Control-Allow-Origin**

**Preflight Request**
  OPTIONS /
  Host: api.example.com
  Origin: https://ww.example.com

**Preflight Response** 
  Access-Control-Allow-Methods: GET, PUT, POST 
  Access-Control-Allow-Headers: 
  Access-Control-Allow-Origin: https://www.example.com 

GET /
Host: api.example.com
Origin: https://www.example.com


# Security
## IAM Permissions
- Create an IAM policy authorization & attach to User/Role
- Authentication = IAM | Authorization = IAM Policy
- Good to provide access within AWS
- Leverages "Sig v4" capability where IAM credentials are in headers
## Resource Policies (similar to Lambda resource policy)
- Allow for cross account access (combined with IAM security)
- Allow for specific source IP address
- Allow for a VPC endpoint

## Cognito User Pools
- Cognito fully manages user lifecycle, token expires automatically
- API gateway verifies identity automatically from AWS Cognito
- No custom implementation required
- Auth = Cognito User Pools | Authz = API Gateway Methods

## LambdaAuthorizer
- Token-based authorizer (bearer-token) - ex JWT or OAuth

## WebSocket
- Two-way interactive communication
- enables **Stateful** application use cases
- GET
- POST
- DELETE
```
{
  "service": "chat",
  "action": "join',
  "data": {
    "room": "room1234"
  }
}
```

## RouteKeyTable
$connect
$diconnect
$default
join
quit
delete