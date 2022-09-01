# PROPOSE

The PROPOSE interaction submits a [proposal](../entities/proposal.md) on-chain to initiate a poll based on the parameters defined in the [proposal](../entities/proposal.md) entity.
A [proposal](../entities/proposal.md) can subject multiple collections to a vote as long as the submitter is the issuer of all subjected collections.
Equippables can also be used as boosts or requirements which do not necessarily need to be issued by the [proposal](../entities/proposal.md) submitter.

## Standard

A PROPOSE interaction is a [batchAll](https://polkadot.js.org/docs/api/cookbook/tx#how-can-i-batch-transactions) call of a system remark and a token transfer.

**`system.remark`**

The format for the system remarks is `0x{bytes(RMRKDAO::PROPOSE::{version}::{html-encoded json})}`
- `version` is the version of RMRK of the subjected collections. (e.g. ~~`1.0.0`~~, `2.0.0`)
- `html-encoded json` is the encoded [proposal](../entities/proposal.md) entity

**`balances.transfer`**

The balance transfer must be equal to or greater than the referenced [custodian](../entities/custodian.md)'s proposalFee. A proposal will be invalidated if it is
not accompanied with an appropriate balance transfer.

## Considerations

`id`: this should be generated randomly to ensure it is unique across all proposals. A proposal will be invalidated if the id matches another proposal and the 
proposalFee to the custodian will be lost.

`options`: there must be a minimum of 2 options to a proposal. Options are totally arbitrary and up to the submitter to communicate the implications of voting on these options.

`snapshot`: in most cases, this field is not needed. However, if an issuer wishes to poll holders at a time that is different than the endDate of the proposal, they
may configure that time with this field. The only restriction to this is that the snapshot must be at a date after the creation of the collection.

`startDate`: a proposal will be invalidated if this date is before the submission of the proposal.

`custodian`: the custodian must be registered on-chain before a proposal references that custodian. While a custodian is technically responsible for tallying votes
and tracking the proposals they are selected for, there are no guarantees that they will fulfil these responsibilities.

`nftWeight`: if this value is false, users will still be able to distribute their NFTs across multiple wallets as a workaround.

`electorate`: if this value is true, the total vote weight toward any of the options will be weighed against the total possible vote weight at the time of the snapshot
given the parameters of the proposal.

## Example

Let's say the issuer of `3208723ec6f65df810-SHELF` wants to ask holders what their favorite color is and assign `DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17` as the custodian:

```
{
	"name": "What is your favorite color?",
	"description": "We want to know our holders’ favorite colors.",
	"collections": ["3208723ec6f65df810-SHELF"],
	"id": "1e6ttkjfvv",
	"options": ["Red", "Blue", "Yellow", "Green", "Orange", "Purple", "Pink"],
	"passingThreshold": 20,
	"startDate": 1654041600,
	"endDate": 1654560000,
	"custodian": "DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17",
	"nftWeight": true,
	"electorate": false
}
```

`DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17` has registered as a custodian with these parameters:

```
{
	"proposalFee": "110000000000",
	"voteFee": "50000000000",
	"recertifyFee": "1000000000000",
	"maxOptions": 100
}
```

So the issuer of `3208723ec6f65df810-SHELF` would prepare the following string:

```
RMRKDAO::PROPOSE::2.0.0::%7B%22custodian%22%3A%22DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17%22%2C%22proposalFee%22%3A%22110000000000%22%2C%22voteFee%22%3A%2250000000000%22%2C%22recertifyFee%22%3A%221000000000000%22%2C%22maxOptions%22%3A100%7D
```

And submit it as a `system.remark` in hexidecimal format accompanied with a balance transfer of 0.11 KSM to `DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17`:

```
0x524d524b44414f3a3a50524f504f53453a3a322e302e303a3a253742253232637573746f6469616e25323225334125323244616f516b4e67786f6267704b6664394e6d45546e5765364643664445526475535452325969464452474b764c313725323225324325323270726f706f73616c466565253232253341253232313130303030303030303030253232253243253232766f74654665652532322533412532323530303030303030303030253232253243253232726563657274696679466565253232253341253232313030303030303030303030302532322532432532326d61784f7074696f6e73253232253341313030253744
```
