# PROPOSE

The PROPOSE interaction submits a [proposal](../entities/proposal.md) on-chain to initiate a poll based on the parameters defined in the [proposal](../entities/proposal.md) entity.
A [proposal](../entities/proposal.md) can subject multiple collections to a vote as long as the submitter is the issuer of all subjected collections.
Equippables can also be used as boosts or requirements which do not necessarily need to be issued by the [proposal](../entities/proposal.md) submitter.

## Standard

A PROPOSE interaction is a [batchAll or batch](https://polkadot.js.org/docs/api/cookbook/tx#how-can-i-batch-transactions) call of a system remark and a token transfer.

**`system.remark`**

The format for the system remarks is `0x{bytes(RMRK::DAO::PROPOSE::{version}::{html-encoded json})}`
- `version` is the version of RMRK of the subjected collections. (e.g. `1.0.0`, `2.0.0`)
- `html-encoded json` is the encoded [proposal](../entities/proposal.md) entity

**`balances.transfer`** or **`balances.transferAll`** or **`balances.transferKeepAlive`**

The balance transfer must be equal to or greater than the referenced [custodian](../entities/custodian.md)'s proposalFee. A proposal will be invalidated if it is
not accompanied with an appropriate balance transfer.

## Considerations

`id`: this should be generated randomly to ensure it is unique across all proposals. A proposal will be invalidated if the id matches another proposal and the 
proposalFee to the custodian will be lost.

`options`: there must be a minimum of 2 options to a proposal. Options are totally arbitrary and up to the submitter to communicate the implications of voting on these options.

`snapshot`: in most cases, this field is not needed. However, if an issuer wishes to poll holders at a time that is different than the endDate of the proposal, they
may configure that time with this field. The only restriction to this is that the snapshot must be at a date after the creation of the collection.

`holdRequirement`: this field uses blocks as its time frame of reference. In most cases, blocks are 6 seconds long but often take longer.

`startDate`: a proposal will be invalidated if this date is before the submission of the proposal.

`custodian`: the custodian must be registered on-chain before a proposal references that custodian. While a custodian is technically responsible for tallying votes
and tracking the proposals they are selected for, there are no guarantees that they will fulfil these responsibilities.

`nftWeight`: if this value is false, users will still be able to distribute their NFTs across multiple wallets as a workaround.

`electorate`: if this value is true, the total vote weight toward any of the options will be weighed against the total possible vote weight at the time of the snapshot
given the parameters of the proposal.

`equippableParameters`: this can only be included if the proposal is for a collection that supports child NFTs and the EQUIP interaction (i.e. RMRK version 2.0.0 or greater). This option is allowed for 2.0.0 NFT collections that might not have BASE functionality - it is the responsibility of the issuer to know the limits of their collections' equippables and the ramification of including this option in a proposal for those collections.

## Example

Let's say the issuer of `3208723ec6f65df810-SHELF` wants to ask holders what their favorite color is and assign `GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM` as the custodian:

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

`GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM` has registered as a custodian with these parameters:

```
{
	"custodian": "GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM",
	"proposalFee": "110000000000",
	"voteFee": "50000000000",
	"recertifyFee": "1000000000000"
}
```

So the issuer of `3208723ec6f65df810-SHELF` would prepare the following string:

```
RMRK::DAO::PROPOSE::2.0.0::%7B%22name%22%3A%22What%20is%20your%20favorite%20color%3F%22%2C%22description%22%3A%22We%20want%20to%20know%20our%20holders%E2%80%99%20favorite%20colors.%22%2C%22collections%22%3A%5B%223208723ec6f65df810-SHELF%22%5D%2C%22id%22%3A%221e6ttkjfvv%22%2C%22options%22%3A%5B%22Red%22%2C%22Blue%22%2C%22Yellow%22%2C%22Green%22%2C%22Orange%22%2C%22Purple%22%2C%22Pink%22%5D%2C%22holdRequirement%22%3A144000%2C%22passingThreshold%22%3A20%2C%22startDate%22%3A1654041600%2C%22endDate%22%3A1654560000%2C%22custodian%22%3A%22GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM%22%2C%22nftWeight%22%3Atrue%2C%22electorate%22%3Afalse%2C%22equippableParameters%22%3A%7B%22thresholdEquippables%22%3A%5B%223208723ec6f65df810-ITEM%22%5D%2C%22threshold%22%3A3%2C%22boostEquippables%22%3A%5B%223208723ec6f65df810-ITEMXRMRK%22%2C%223208723ec6f65df810-ITEMXEVRLOOT%22%5D%2C%22boost%22%3A0.25%7D%7D
```

And submit it as a `system.remark` in hexidecimal format accompanied with a balance transfer of 0.11 KSM to `GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM`:

```
0x524d524b3a3a44414f3a3a50524f504f53453a3a322e302e303a3a2537422532326e616d65253232253341253232576861742532306973253230796f75722532306661766f72697465253230636f6c6f722533462532322532432532326465736372697074696f6e253232253341253232576525323077616e74253230746f2532306b6e6f772532306f7572253230686f6c646572732545322538302539392532306661766f72697465253230636f6c6f72732e253232253243253232636f6c6c656374696f6e732532322533412535422532323332303837323365633666363564663831302d5348454c46253232253544253243253232696425323225334125323231653674746b6a6676762532322532432532326f7074696f6e73253232253341253542253232526564253232253243253232426c756525323225324325323259656c6c6f77253232253243253232477265656e2532322532432532324f72616e6765253232253243253232507572706c6525323225324325323250696e6b253232253544253243253232686f6c64526571756972656d656e7425323225334131343430303025324325323270617373696e675468726573686f6c64253232253341323025324325323273746172744461746525323225334131363534303431363030253243253232656e644461746525323225334131363534353630303030253243253232637573746f6469616e253232253341253232476f53746272765532795844584c4a4d433271486b767579366171486b31774c4c417566687438336435455355524d2532322532432532326e667457656967687425323225334174727565253243253232656c6563746f7261746525323225334166616c736525324325323265717569707061626c65506172616d65746572732532322533412537422532327468726573686f6c6445717569707061626c65732532322533412535422532323332303837323365633666363564663831302d4954454d2532322535442532432532327468726573686f6c6425323225334133253243253232626f6f737445717569707061626c65732532322533412535422532323332303837323365633666363564663831302d4954454d58524d524b2532322532432532323332303837323365633666363564663831302d4954454d584556524c4f4f54253232253544253243253232626f6f7374253232253341302e3235253744253744
```
