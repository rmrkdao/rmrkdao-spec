# UNDER DEVELOPMENT
# RMRK DAO Specifications and Standards
An extension of the [RMRK 2.0.0 specifications and standards](https://github.com/rmrk-team/rmrk-spec#extending-the-standard).

## Interactions
#### Collection Issuers
[PROPOSE](./interactions/PROPOSE.md): allows a collection issuer to create a new proposal pertaining to their collection on-chain.

RECERTIFY: recertify an already counted proposal with a different custodian

CANCEL: cancel an on-going proposal.

#### NFT Holders

[VOTE](./interactions/VOTE.md): vote on an existing on-chain proposal.

DELEGATE: delegate your vote weight for a given proposal to another wallet.

#### RMRK DAO Custodians

[REGISTER](./interactions/REGISTER.md): register as a RMRK DAO custodian and declare your fees.

[DEREGISTER](./interactions/DEREGISTER.md): remove your registration as a RMRK DAO custodian

[SUBMIT](./interactions/SUBMIT.md): Submit the results of a proposal.

REJECT: reject an assignment by refunding the proposer.

## Entities
[Proposal](./entities/proposal.md): an on-chain entity defining the components of a proposal.

[Custodian](./entities/custodian.md): an on-chain entity defining the wallet and fees of a custodian.

[Result](./entities/result.md): the counts and outcome of a proposal.
