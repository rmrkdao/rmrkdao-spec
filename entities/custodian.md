# Custodian

A custodian is the entity that is responsible for the tally of a [proposal](proposal.md)'s votes. A proposal must have a custodian assigned to it.
A registered custodian is responsible for tracking when they are assigned to a proposal and is also responsible for counting the vote according to the RMRK::DAO standard.

```
{
	"custodian": {
		"type": "string",
		"value": "the KSM wallet of the custodian. Must be the same wallet as the one submitting the REGISTER interaction. For example: GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM."
	},
	"proposalFee": {
		"type": "string",
		"value": "the amount of Planck KSM (1 Planck KSM = 10^-12 KSM) required for a collection issuer to pay a custodian to assign a proposal to that custodian. For example: 100000000000 is equal to 0.1 KSM."
	},
	"voteFee": {
		"type": "string",
		"value": "the amount of Planck KSM required for a voter to pay a custodian to ensure their vote is counted. For example 10000000000 = is equal to 0.01 KSM."
	},
	"recertifyFee": {
		"type": "string",
		"value": "the amount of Planck KSM required for a collection issuer to pay a custodian to recertify a count."
	}
}
```

## Example

```
{
	"custodian": "GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM",
	"proposalFee": "110000000000",
	"voteFee": "50000000000",
	"recertifyFee": "1000000000000"
}
```
