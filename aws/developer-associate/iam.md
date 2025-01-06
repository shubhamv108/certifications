
Global

Group {
	List<User>
	List<Policy>
}

User {
	List<Group>
	List<Policy>

	getPolicies() {
		policies + List<groups::policies>
	}
}

/**
 * Permissions: apply Least priveledge Principle
 */
Policy {
	version
	List<Statement>
}

Statement {
	sid
	List<Effect{Allow, Deny}> 
	List<Principle>
	List<Action>
	resource
	condition
}

Effect {

}

Action {
	service
	operation
}

Principle {
	account
	user
	role
}



Policy
AdministratorAccess


Role {
	List<Policy>
}

### Security Tools
	1. CredentialsReport (Account Level)
	2. Access Advisor (User Level)


### Guidelines & BestPractices
	1. Don't use root account except for AWs account setup
	2. one physical user = 1 AWS user
	3. Assign users to group & assign permissions to group
	4. create a string password policy
	5. use & enforce MFA
	6. Create & use roles for giving permission to aws services
	7. use access key for programmatic access
	8. Audot permisisons of your account using iam credentials report & iam Access Advisor
	9. never share iam users & access keys


## Shared Responsibility Model
#### AWS
	1. Infra (global N/w security)
	2. Config & vulnerability analysis
	3. compliance validation
#### User
	1. Users, Groups, Roles, Policies management & monitoring
	2. Enable MFA on all accounts
	3. Rotate all your keys often
	4. use IAM tools to appropriate permissions
	5. Analyze access patterns & review permissions 