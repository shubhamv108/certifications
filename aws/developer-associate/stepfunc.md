Workflows as State machines
- written in json
- visualization of workflow and execution of workflow, as well as history

- to start workflow use sdk api  call, apii gateway, event brodge (cloudwatch event)
- has tasks
- Task State
	I - Invoke one aws service
		- invoke lambda
		- runa  aws batch job
		- run an ecs task
		- insert to dynamodb
		- publish message to sns, sqs
		- launch another step func workflow
	- Run a one activity
		- ec2 ECS, om premises
		- activity polls step func for work
		- activities ends results back to step funcs

# states
1. Choice 
	- test a cond to send to a branch
2. Fail or succed state
	- Stop execution with fail or success
3. Pass state
	- simple to pass its i/p to its o/p or inject some fixed data, without performing work
4. Wait state
	- provide a delay for a certain amount of time or until a pecified time/date
	map state - dynamically iterate steps
5. parrallel state - to begin parallel branches of execution


## Error Code
	1. States.ALL
	2. state.Timeout
	3. State.TaskFailed


# Standard
MaxDuration: 1year
ExecutionModel: Exacty once execution
Execution Rate: Over 2000/second
Execution History: upto 90 days using Cloudwatch
Pricing: # of state transitions
use cases: Non idempotent actions (eg. processing payments)

## Express
Short Upto 5 mins
Over 100k / sec
cloudwatch logs
Pricing: # executions, duration, memory consumption
usecases: iot, data ingestion, streaming data, mobile app backends
Asyn: At-least once gaurantee
Syn: At most once gaurantee