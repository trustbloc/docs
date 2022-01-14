# Witness Policy

An administrator can define and configure witness policy per domain. If configured the witness policy 
is stored into configuration database "orb-config" under "witness-policy" key. 
The witness policy is cached on the server with periodic cache updates from the database. 
Default witness policy cache expiry period has been set to 30 seconds.

##Witness Policy Rules

###OutOf

OutOf rule defines minimum number of witnesses required to provide proof in order to satisfy witness policy. 

Syntax:
OutOf(minimumWitnesses,witnessType)

First parameter must be zero or positive integer and it denotes minimum number of witnesses required 
to provide proof in order to satisfy witness policy.

Second parameter denotes type of witness. Supported witness types are batch and system.

Example:
OutOf(2,system) 

This rule means that proofs from at least 2 system witnesses are required in order to satisfy witness policy.

###MinPercent

MinPercent rule defines minimum percent of witnesses required to provide proof in order to satisfy witness policy. 

Syntax:
MinPercent(minimumWitnessesPercent,witnessType)

First parameter must be an integer between 0 and 100 and it denotes minimum percent of witnesses 
required to provide proof in order to satisfy witness policy.

Second parameter denotes type of witness. Supported witness types are batch and system.

Example:
MinPercent(50,batch)

This rule means that proofs from at least 50% of batch witnesses are required in order to satisfy witness policy.

###LogRequired

LogRequired rule means that all witnesses have to be configured with supported witness log. 

Syntax:
LogRequired

###Combining Rules

Rules can be combined by using operators. Supported operators: AND, OR.

Example:
MinPercent(50,batch) AND MinPercent(50,system)

This rule means that proofs from at least 50% of batch witnesses and proofs from at least 50% of system witnesses 
are required in order to satisfy witness policy.

Additional examples:
MinPercent(50,batch) OR MinPercent(50,system)
OutOf(3,system) AND OutOf(1,batch) LogRequired

###Default Policy 

If the witness policy has not been configured the system will default to 100% batch and 100% system witnesses policy.
      		
###Configuring Witness Policy

Witness policy can be configured by posting witness configuration rule to domain endpoint "/policy".



