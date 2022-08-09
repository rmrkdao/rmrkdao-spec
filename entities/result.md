# Result

A result entity is the certified vote count of a proposal from a custodian, whether they were assigned the proposal or they were assigned to
[RECERTIFY](../interactions/RECERTIFY.md).

```
{
	"id": {
		"type": "string",
		"value": "the proposal id"
	},
	"count": {
		"type": "array",
		"value": [
			{
			"option 1": {
				"type": "number",
				"value": "the count for option 1 given the parameters of the proposal."
				}
			},
			{
			"option 2": {
				"type": "number",
				"value": "the count for option 2 given the parameters of the proposal."
				}
			},
			{"...": "..."},
			{
			"option n": {
				"type": "number",
				"value": "the count for option n given the parameters of the proposal."
				}
			}
		]
	},
	"winningOptions": {
		"type": "array",
		"value": "the option(s) with the most votes. If none of the options meet the passingThreshold, this should be an empty array. More than one value implies a tie."
	},
	"electorate": {
		"type": "boolean",
		"value": "if true, votes are weighted against the full potential of the electorate given the parameters of the proposal. If false, votes are weighted against the turnout."
	},
	"thresholdDenominator": {
		"type": "number",
		"value": "if electorate is false, this is simply the sum of all options' counts.
		If electorate is true, this value should be the the maximum possible turnout if every NFT in the collection(s) voted and were boosted insofar that
		equippables exist. The electorate vote potential is a non-trivial calculation on the `2.0.0` version of RMRK. Custodians ought to carefully consider how to
		calculate the fully optimized electorate vote potential."
	},
	"recertify": {
		"type": "boolean",
		"value": "if false, this result is being submitted because the custodian was originally assigned to the proposal. If true, the custodian was assigned to recertify the proposal."
	}
}
```

## Example

Let's say [this proposal example](proposal.md#example) has been completed. The results might look something like this:

```
{
	"id": "1e6ttkjfvv",
	"count": [
		{"Red": 50.5},
		{"Blue": 200.25},
		{"Yellow": 25},
		{"Green": 150.25},
		{"Orange": 10.5},
		{"Purple": 300},
		{"Pink": 700.25}
	],
	"winningOptions": ["Pink"],
	"electorate": false,
	"thresholdDenominator": 1436.75,
	"recertify": false
}
```