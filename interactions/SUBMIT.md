# SUBMIT

The SUBMIT interaction is how a [custodian](../entities/custodian.md) submits the [result](../entities/result.md) of a [proposal](../entities/proposal.md) on-chain.

## Standard

A SUBMIT interaction is a `system.remark` whose format is `0x{bytes(RMRKDAO::SUBMIT::{version}::{url-encoded json})}`

- `version` is the version of RMRK of the subjected collections (e.g. `2.0.0`)
- `url-encoded json` is the encoded [result](../entities/result.md) entity

## Implications

If for some reason a custodian submits multiple SUBMIT interactions for the same proposal in one batch, the result contained in the remark that has the lowest event index in the block should be
associated with the computed result ID.

It is up to the proposal issuer to determine which results to use in the case a custodian submits multiple SUBMIT interactions for the same proposal over time. Custodians should avoid
submiting multiple results for the same proposal since that would reflect poorly on their reputation.
