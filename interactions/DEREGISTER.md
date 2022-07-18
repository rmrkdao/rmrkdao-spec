# DEREGISTER

The DEREGISTER interactions removes a [custodian](../entities/custodian.md) from the pool of active custodians on the specified RMRK version.

## Standard

The DEREGISTER interaction is a `system.remark` whose format is `0x{bytes(RMRK::DAO::DEREGISTER::{version})}`

- `version` is the version of RMRK of the subjected collections (e.g. `1.0.0`, `2.0.0`)

A cool-off period of 24 hours is required before a custodian can [REGISTER](./register.md) again.

## Considerations

If in the unlikely case a [PROPOSE](./propose.md) or [RECERTIFY](./recertify.md) interaction assigns a custodian in the same block that custodian DEREGISTERS,
that custodian is still obligated to count the votes.

## Example

Let's assume `GoStbrvU2yXDXLJMC2qHkvuy6aqHk1wLLAufht83d5ESURM` is a registered custodian on versions `1.0.0` and `2.0.0`. If they would like to stop counting
`1.0.0` votes, they would submit:

```
RMRK::DAO::DEGREGISTER::1.0.0
```

This would not affect their `2.0.0` registration.
