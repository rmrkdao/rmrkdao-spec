# Proposal

A proposal is an on-chain entity defining the mechanisms and parameters of a poll for a group of NFT holders in one or more collections.

```
{
	"name": {
		"type": "string",
		"value": "the name of the proposal, i.e. the question you are asking."
	},
	"description": {
		"type": "string",
		"value": "a description for the proposal, background information, reasoning, etc."
	},
	"collections": {
		"type": "array",
		"value": "an array of collection ids that are the subjects of the proposal. The proposal creator must be the issuer of all collections in this array"
	},
	"id": {
		"type": "string",
		"value": "a 10 digit case insensitive alpha-numeric string used to identify the proposal.
		This should be generated randomly and should be unique across all proposals.
		This string cannot contain any special characters. For example 1e6ttkjfvv is acceptable but 1.^jta_$4% is not."
	},
	"options": {
		"type": "array",
		"value": ["option 1","option 2", "..." , "option n (minimum of 2)"]
	},
	"?snapshot": {
		"type": "datetime",
		"value": "(optional) the Unix timestamp when the owner snapshot should be taken. This can be any date after the creation of the collection. If omitted, the snapshot date = endDate."
		},
	"?holdRequirement": {
		"type": "number",
		"value": "(optional) the number of blocks before the snapshot date that a holder needs to own an NFT in order to vote. If omitted, this value defaults to 1."
	},
	"?passingThreshold": {
		"type": "number",
		"value": "(optional) the percentage of the turnout vote that an option needs in order to pass. If omitted, a simple majority is used."
	},
	"?startDate": {
		"type": "datetime",
		"value": "(optional) the Unix timestamp when voting starts. If omitted, the timestamp of the block of the proposal creation will be used as the start date."
	},
	"endDate": {
		"type": "datetime",
		"value": "the Unix timestamp when voting ends, must be at least 1 minute after startDate"
	},
	"custodian": {
		"type": "string",
		"value": "the KSM wallet of the custodian of the proposal, i.e. the individual responsible for the count. This is the wallet where the creation/voting fees for the proposal will be sent to. Must be a registered custodian."
	},
	"nftWeight": {
		"type": "boolean",
		"value": "if true 1 NFT = 1 vote, if false 1 wallet = 1 vote."
	},
	"electorate": {
		"type": "boolean",
		"value": "If true, the passing threshold is weighed against the entire electorate's vote weight.
		For example, if there are 100 possible votes in the electorate and there are three options (A, B, C) to a proposal, if this key is true and passingThreshold is omitted, then in order for option A to pass there needs to be greater than 50 votes.
  		If the result of this vote is A: 40, B: 20, C: 10, D: 30, none of the options pass. However in this example, if this key is false, option A would pass.
  		The electorate vote weight depends on a couple parameters.
  		If there are 50 unique owners of a collection of 100 NFTs, the electorate's vote weight would equal 100 if nftWeight = true, and 50 if false.
  		If there are boosts in a proposal, the maximum potential of vote weight at the time of the snapshot will be considered the weight of the electorate.
  		For example, if there are 100 NFTs in the proposal's respective collection and 1000 possible equippables that give a 0.5 boost, but only 2 slots for these NFTs in the base resource assigned to the NFTs in the proposal's respective collection, the electorate's total possible vote weight would be equal to 200." 
	},
	"?equippableParameters": {
		"type": "object",
		"value": {
			"?requiredEquippables": {
				"type": "array",
				"value": "an array of collection ids that all must be equipped (i.e. not pending) to an NFT in the proposal's respective collection."
			},
			"?thresholdEquippables": {
				"type": "array",
				"value": "an array of collection ids that, when equipped to an NFT in the proposal's respective collection, count toward a threshold required for that NFT's vote to count."
			},
			"?threshold": {
				"type": "number",
				"value": "the minimum threshold required of equipped NFTs in the thresholdEquippables array for an NFT's vote to count. If omitted, this value defaults to 0."
			},
			"?boostEquippables": {
				"type": "array",
				"value": "an array of collection ids that, when equipped to an NFT in the proposal's respective collection, count toward a voting boost."
			},
			"?boost": {
				"type": "number",
				"value": "the vote boost per NFT equipped in the boostEquippables. If omitted, this value defaults to 1."
			}
		}
	}
}
```
Example:
```
{
	"name": "What is your favorite color?",
	"description": "We want to know our holders’ favorite colors.",
	"collections": ["3208723ec6f65df810-SHELF"],
	"id": "1e6ttkjfvv",
	"options": ["Red", "Blue", "Yellow", "Green", "Orange", "Purple", "Pink"],
	"holdRequirement": 144000,
	"passingThreshold": 20,
	"startDate": 1654041600,
	"endDate": 1654560000,
	"custodian": "GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM",
	"nftWeight": true,
	"electorate": false,
	"equippableParameters": {
		"thresholdEquippables": ["3208723ec6f65df810-ITEM"],
		"threshold": 3,
		"boostEquippables": ["3208723ec6f65df810-ITEMXRMRK", "3208723ec6f65df810-ITEMXEVRLOOT"],
		"boost": 0.25
	}
}
```
This 20% minimum majority of turnout proposal begins on June 1st, 2022 at 00:00 UTC and ends June 7th, 2022 at 00:00 UTC. Since Kusama blocks occur every 6 seconds:

- The first block whose timestamp exceeds or equals the startDate is the inclusive start block of the proposal.
- The first block whose timestamp exceeds or equals the endDate is the inclusive end block of the proposal.

The snapshot will be taken at the endDate block and holders are required to hold their NFTs for at least 144000 blocks before the snapshot (~10 days). Holders are required to have at least 3 NFTs from the collection `3208723ec6f65df810-ITEM` equipped in order for their vote to count. For every NFT equipped from either `3208723ec6f65df810-ITEMXRMRK` or `3208723ec6f65df810-ITEMXEVRLOOT` will add a 0.25 vote boost to their vote. And each NFT is given voting consideration.
