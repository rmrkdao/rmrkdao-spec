# REGISTER

The REGISTER interaction registers a wallet as a [custodian](../entities/custodian.md) on-chain.
This signals to the ecosystem that the wallet operator is willing to count the votes of [proposals](../entities/proposal.md)
that are assigned to them. Collection issuers need to ensure they are selecting a reputable custodian.
However, if a custodian does not follow through on a count, the collection issuer can always issue
a [RECERTIFY](./recertify.md) interaction to assign a different custodian to count the votes of a proposal.

## Standard

A REGISTER interaction is a `system.remark` whose format is `0x{bytes(RMRK::DAO::REGISTER::{version}::{html-encoded json})}`

- `version` is the version of RMRK of the subjected collections (e.g. `1.0.0`, `2.0.0`)
- `html-encoded json` is the encoded [custodian](../entities/custodian.md) entity

A custodian may declare separate fees for separate versions of the RMRK standard. Custodians
do not need to register for all versions of RMRK if they choose not to.

**A CUSTODIAN CANNOT REGISTER FOR THE SAME STANDARD MORE THAN ONCE PER 14,400 BLOCKS (~24 HOURS), EVEN IF THEY [DEREGISTER](./deregister.md)**.

## Considerations

If a custodian is already registered and submits a REGISTER interaction for the same version of RMRK, their fees are updated as long as their last REGISTER interaction
for that standard occurred at least 14,400 blocks ago. **THE `proposalFee` AND THE `voteFee` OF A CUSTODIAN AT THE MOMENT OF BEING ASSIGNED A PROPOSAL SHALL APPLY TO
THAT PROPOSAL AND ITS VOTES. SAME APPLIES TO RECERTIFY ASSIGNMENTS** This prevents any sort of bait and switch.

## Example

Let's say the [custodian entity example](../entities/custodian.md#example) would like to register on both the `1.0.0` and `2.0.0` standards using the same fees. The wallet
`GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM` would submit two separate remarks:
```
RMRK::DAO::REGISTER::1.0.0::%7B%22custodian%22%3A%22GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM%22%2C%22proposalFee%22%3A%22110000000000%22%2C%22voteFee%22%3A%2250000000000%22%2C%22recertifyFee%22%3A%221000000000000%22%7D
```

```
RMRK::DAO::REGISTER::2.0.0::%7B%22custodian%22%3A%22GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM%22%2C%22proposalFee%22%3A%22110000000000%22%2C%22voteFee%22%3A%2250000000000%22%2C%22recertifyFee%22%3A%221000000000000%22%7D
```
