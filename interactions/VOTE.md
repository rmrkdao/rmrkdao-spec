# VOTE

The VOTE interaction allows a user to vote on a proposal as long as they own the necessary NFTs in the proposal's respective collections and meet the proposal's requirements.

## Standard

A VOTE interaction is a [batchAll or batch](https://polkadot.js.org/docs/api/cookbook/tx#how-can-i-batch-transactions) call of a system remark and a token transfer.

**`system.remark`**

The format for the system remarks is `0x{bytes(RMRK::DAO::VOTE::{proposal id}::{option})}`
- `proposal id` is the `id` field in the [proposal](../entities/proposal.md) entity.
- `option` is one of the options in the `options` array in the [proposal](../entities/proposal.md) entity.

**`balances.transfer`** or **`balances.transferAll`** or **`balances.transferKeepAlive`**

The balance transfer must be equal to or greater than the referenced [custodian](../entities/custodian.md)'s voteFee. A proposal will be invalidated if it is
not accompanied with an appropriate balance transfer.

## Example

Let's say a user wants to vote "Red" on [this example proposal](PROPOSE.md#example). This user owns one NFT in the collection `3208723ec6f65df810-SHELF` with three NFTs
from `3208723ec6f65df810-ITEM` and one NFT from `3208723ec6f65df810-ITEMXRMRK` equipped to it. They would submit the following `system.remark`
accompanied with a balance transfer of 0.05 KSM to `GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM`:

```
RMRK::DAO::VOTE::rF5yB::Red
```

Their vote would be given a vote weight of 1.25 since they meet a boost requirement.
