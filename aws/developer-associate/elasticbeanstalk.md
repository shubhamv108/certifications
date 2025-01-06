## DeploymentOptions
1. All at once
2. Rolling
3. Rolling with additional batches: new instances to move to the batch
4. Immutable: 
	- new instances in new ASG, 
	- swaps all instances in everything is healthy
	- zero downtime
	- quick rollbacks
5. Blue Green: create a new env (stage env) and switch over when ready
	- zero downtime
	- Route53 can be used with seighted policies to redirect a little bit of traffic to sage env
	- "swap urls" when done with new environment testing


6. Traffic Splitting: canary testing - send a a small % of traffic to new deployment 