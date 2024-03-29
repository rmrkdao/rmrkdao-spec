# VOTE

The VOTE interaction allows a user to vote on a proposal as long as they own the necessary NFTs in the proposal's respective collections and meet the proposal's requirements.

## Standard

A VOTE interaction is a [batchAll](https://polkadot.js.org/docs/api/cookbook/tx#how-can-i-batch-transactions) call of a system remark and a token transfer.

**`system.remark`**

The format for the system remarks is `0x{bytes(RMRKDAO::VOTE::{version}::{proposal id}::{option index})}`
- `version` is the version of RMRK of the subjected collections (e.g. 2.0.0)
- `proposal id` is the `id` field in the [proposal](../entities/proposal.md) entity.
- `option index` is one of the option indexes in the `options` object in the [proposal](../entities/proposal.md) entity.

**`balances.transfer`**

The balance transfer must be equal to or greater than the referenced [custodian](../entities/custodian.md)'s voteFee. A proposal will be invalidated if it is
not accompanied with an appropriate balance transfer.

## Considerations

If the same user votes multiple times on the same proposal during the voting period, the last vote before the proposal's endDate will be considered the final vote for that user.

The voting weight a user has on any given proposal is not finalized until that proposal's snapshot. It is the responsibility of the voter to be aware of the parameters
of a proposal before voting to understand how their vote weight might be affected by their actions.

## Example

Let's say a user wants to vote "Red" on [this example proposal](PROPOSE.md#example). This user owns one NFT in the collection `3208723ec6f65df810-SHELF`. They would submit the following `system.remark` accompanied with a balance transfer of 0.05 KSM to `DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17`:

```
RMRKDAO::VOTE::2.0.0::1e6ttkjfvv::0
```

Note, the option "Red" is nowhere in the vote interaction - only its index in the options object in the proposal.
