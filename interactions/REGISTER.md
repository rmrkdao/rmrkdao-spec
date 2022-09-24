# REGISTER

The REGISTER interaction registers a wallet as a [custodian](../entities/custodian.md) on-chain.
This signals to the ecosystem that the wallet operator is willing to count the votes of [proposals](../entities/proposal.md)
that are assigned to them. Collection issuers need to ensure they are selecting a reputable custodian.

## Standard

A REGISTER interaction is a `system.remark` whose format is `0x{bytes(RMRKDAO::REGISTER::{version}::{url-encoded json})}`

- `version` is the version of RMRK of the subjected collections (e.g. `2.0.0`)
- `url-encoded json` is the encoded [custodian](../entities/custodian.md) entity

**A CUSTODIAN CANNOT REGISTER FOR THE SAME STANDARD MORE THAN ONCE PER 24 HOURS (every 86,400,000 milliseconds), EVEN IF THEY [DEREGISTER](DEREGISTER.md)**.

## Considerations

If a custodian is already registered and submits a REGISTER interaction for the same version of RMRK, their fees are updated as long as their last REGISTER interaction
for that standard occurred at least 86,400,000 milliseconds. **THE `proposalFee` AND THE `voteFee` OF A CUSTODIAN AT THE MOMENT OF BEING ASSIGNED A PROPOSAL SHALL APPLY TO THAT PROPOSAL AND ITS VOTES. SAME APPLIES TO RECERTIFY ASSIGNMENTS** This prevents any sort of bait and switch.

## Example

Let's say the [custodian entity example](../entities/custodian.md#example) would like to register. The wallet
`DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17` would submit the remark:

```
RMRKDAO::REGISTER::2.0.0::%7B%22proposalFee%22%3A%22110000000000%22%2C%22voteFee%22%3A%2250000000000%22%2C%22recertifyFee%22%3A%221000000000000%22%2C%22maxOptions%22%3A100%7D
```
