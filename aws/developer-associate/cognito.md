- give users an identity to interact without web or mobile application
- Cognito User pools
  - sign in functioinality for user apps
  - Integrate with api gateway 7 application load balancer
- Cognito identity pool (Federated Identity):
  - Provide AWS credentals to users so they can access AWS resources directly
  - Integrate with Cognito user Pools as an identity provider

# CognitoUserPools
- serverless db of user for web & mobile
- simple login username/ password
- password reset
- email & phone verification
- MFA
- Federated identities: users from Facebook, Google, SAML
- Block users if credential compromised
- Login sends back JWT
- Integrate with API gateway & ALB
### Hosted UI Custom Domain
- create ACm certificate

### AdaptiveAuthentication
- sign in blocked or require MFA, based on rosk score  