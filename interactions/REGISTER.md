# REGISTER

The REGISTER interaction registers a wallet as a [custodian](../entities/custodian.md) on-chain.
This signals to the ecosystem that the wallet operator is willing to count the votes of [proposals](../entities/proposal.md)
that are assigned to them. Collection issuers need to ensure they are selecting a reputable custodian.

## Standard

A REGISTER interaction is a `system.remark` whose format is `0x{bytes(RMRK::DAO::REGISTER::{version}::{html-encoded json})}`

- `version` is the version of RMRK of the subjected collections (e.g. ~~`1.0.0`~~, `2.0.0`)
- `html-encoded json` is the encoded [custodian](../entities/custodian.md) entity

## Example

Let's say the [custodian entity example](../entities/custodian.md#example) would like to register on the `2.0.0` standard. The wallet
`DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17` would submit the remark:

```
RMRK::DAO::REGISTER::2.0.0::%7B%22custodian%22%3A%22DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17%22%2C%22proposalFee%22%3A%22110000000000%22%2C%22voteFee%22%3A%2250000000000%22%2C%22recertifyFee%22%3A%221000000000000%22%2C%22maxOptions%22%3A100%7D
```
