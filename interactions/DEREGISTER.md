# DEREGISTER

The DEREGISTER interactions removes a [custodian](../entities/custodian.md) from the pool of active custodians on the specified RMRK version.

## Standard

The DEREGISTER interaction is a `system.remark` whose format is `0x{bytes(RMRKDAO::DEREGISTER::{version})}`

- `version` is the version of RMRK of the subjected collections (e.g. ~~`1.0.0`~~, `2.0.0`)

A cool-off period of 24 hours is required before a custodian can [REGISTER](REGISTER.md) again.

## Considerations

If in the unlikely case a [PROPOSE](PROPOSE.md) or [RECERTIFY](RECERTIFY.md) interaction assigns a custodian in the same block that custodian DEREGISTERS,
that custodian is still obligated to count the votes.

## Example

Let's assume `DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17` is a registered custodian on versions ~~`1.0.0`~~ and `2.0.0`. If they would like to stop counting
`2.0.0` votes, they would submit:

```
RMRKDAO::DEGREGISTER::2.0.0
```
