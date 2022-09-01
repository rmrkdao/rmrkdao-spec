# PROPOSE

The PROPOSE interaction submits a [proposal](../entities/proposal.md) on-chain to initiate a poll based on the parameters defined in the [proposal](../entities/proposal.md) entity.
A [proposal](../entities/proposal.md) can subject multiple collections to a vote as long as the submitter is the issuer of all subjected collections.
Equippables can also be used as boosts or requirements which do not necessarily need to be issued by the [proposal](../entities/proposal.md) submitter.

## Standard

A PROPOSE interaction is a [batchAll](https://polkadot.js.org/docs/api/cookbook/tx#how-can-i-batch-transactions) call of a system remark and a token transfer.

**`system.remark`**

The format for the system remarks is `0x{bytes(RMRKDAO::PROPOSE::{version}::{url-encoded json})}`
- `version` is the version of RMRK of the subjected collections. (e.g. ~~`1.0.0`~~, `2.0.0`)
- `url-encoded json` is the encoded [proposal](../entities/proposal.md) entity

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
	"startDate": 1654041600000,
	"endDate": 1654560000000,
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
RMRKDAO::PROPOSE::2.0.0::%7B%22name%22%3A%22What%20is%20your%20favorite%20color%3F%22%2C%22description%22%3A%22We%20want%20to%20know%20our%20holders%E2%80%99%20favorite%20colors.%22%2C%22collections%22%3A%5B%223208723ec6f65df810-SHELF%22%5D%2C%22id%22%3A%221e6ttkjfvv%22%2C%22options%22%3A%5B%22Red%22%2C%22Blue%22%2C%22Yellow%22%2C%22Green%22%2C%22Orange%22%2C%22Purple%22%2C%22Pink%22%5D%2C%22passingThreshold%22%3A20%2C%22startDate%22%3A1654041600000%2C%22endDate%22%3A1654560000000%2C%22custodian%22%3A%22DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17%22%2C%22nftWeight%22%3Atrue%2C%22electorate%22%3Afalse%7D
```

And submit it as a `system.remark` in hexidecimal format accompanied with a balance transfer of 0.11 KSM to `DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17`:

```
0x524d524b44414f3a3a50524f504f53453a3a322e302e303a3a2537422532326e616d65253232253341253232576861742532306973253230796f75722532306661766f72697465253230636f6c6f722533462532322532432532326465736372697074696f6e253232253341253232576525323077616e74253230746f2532306b6e6f772532306f7572253230686f6c646572732545322538302539392532306661766f72697465253230636f6c6f72732e253232253243253232636f6c6c656374696f6e732532322533412535422532323332303837323365633666363564663831302d5348454c46253232253544253243253232696425323225334125323231653674746b6a6676762532322532432532326f7074696f6e73253232253341253542253232526564253232253243253232426c756525323225324325323259656c6c6f77253232253243253232477265656e2532322532432532324f72616e6765253232253243253232507572706c6525323225324325323250696e6b25323225354425324325323270617373696e675468726573686f6c64253232253341323025324325323273746172744461746525323225334131363534303431363030303030253243253232656e644461746525323225334131363534353630303030303030253243253232637573746f6469616e25323225334125323244616f516b4e67786f6267704b6664394e6d45546e5765364643664445526475535452325969464452474b764c31372532322532432532326e667457656967687425323225334174727565253243253232656c6563746f7261746525323225334166616c7365253744
```
