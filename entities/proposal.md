# Proposal

A proposal is an on-chain entity defining the mechanisms and parameters of a poll for a group of NFT holders in one or more collections.

The keys for all values in the options object must be numeric integers, each less than the MAX_SAFE_INTEGER in javascript (i.e. 9007199254740991) and should not be duplicates of each other. The keys cannot contain letters, spaces, or special characters. The integer used is otherwise arbitrary. Due to `JSON.parse()` behavior, if there are duplicate keys the last duplicate key in the object will be used.

```json
{
  "name": {
    "type": "string",
    "value": "the name of the proposal, i.e. the question you are asking. Limit 10,000 characters."
  },
  "description": {
    "type": "string",
    "value": "a description for the proposal, background information, reasoning, etc. Limit 10,000 characters."
  },
  "collections": {
    "type": "array",
    "value": "an array of collection ids that are the subjects of the proposal. The proposal creator must be the issuer of all collections in this array"
  },
  "id": {
    "type": "string",
    "value": "this must be a 10 digit case sensitive alpha-numeric string [a-zA-Z0-9]{10} used to identify the proposal (62^10 possible ids) and must be unique across all proposals."
  },
  "options": {
    "type": "object",
    "value": {
      "0": {
        "type": "string",
        "value": "option 1 (limit 10,000 characters)."
      },
      "1": {
        "type": "string",
        "value": "option 2 (limit 10,000 characters)"
      },
      {"...": "..."},
      "n-1": {
        "type": "string",
        "value": "option n (limit 10,000 characters)"
      }
  },
  "?snapshot": {
    "type": "number",
    "value": "(optional) the Unix timestamp in milliseconds when the owner snapshot should be taken. This can be any date after the creation of the collection. If omitted, thesnapshot date = endDate. Limit 365 days after endDate."
  },
  "?passingThreshold": {
    "type": "number",
    "value": "(optional) the percentage of the turnout vote that an option needs in order to pass (e.g. a percentage of 20 means 20%). If omitted, a simple majority is used."
  },
  "?startDate": {
    "type": "number",
    "value": "(optional) the Unix timestamp in milliseconds when voting starts. If omitted, the timestamp of the block of the proposal creation will be used as the start date. Limit 365 days after date of proposal submission on-chain."
  },
  "endDate": {
    "type": "number",
    "value": "the Unix timestamp in milliseconds when voting ends, must be at least 1 minute after startDate and at most 365 days after startDate"
  },
  "custodian": {
    "type": "string",
    "value": "the KSM wallet of the custodian of the proposal, i.e. the individual responsible for the count. This is the wallet where the creation/voting fees for the proposal will be sent to. Must be a registered custodian."
  },
  "nftWeight": {
    "type": "boolean",
    "value": "if true 1 NFT = 1 vote, if false 1 wallet = 1 vote."
  },
  "electorate": {
    "type": "boolean",
    "value": "If true, the passing threshold is weighed against the entire electorate's vote weight.\n\nFor example, if there are 100 possible votes in the electorate and there are three options (A, B, C) to a proposal, if this key is true and passingThreshold is omitted, then in order for option A to pass there needs to be greater than 50 votes. If the result of this vote is A: 40, B: 20, C: 10, D: 30, none of the options pass. However in this example, if this key is false, option A would pass. The electorate vote weight depends on a couple parameters. If there are 50 unique owners of a collection of 100 NFTs, the electorate's vote weight would equal 100 if nftWeight = true, and 50 if false." 
  }
}
```
## Example

A proposal entity has two states: unconsolidated and consolidated. The issuer submits the proposal entity as an unconsolidated entity. Once it is processed, assuming it meets the spec, it becomes consolidated. Here's an example of a proposal in both states:

### Unconsolidated
```json
{
  "name": "What is your favorite color?",
  "description": "We want to know our holders’ favorite colors.",
  "collections": ["3208723ec6f65df810-SHELF"],
  "id": "1e6ttkjfvv",
  "options": {
    "0": "Red",
    "1": "Blue",
    "2": "Yellow",
    "3": "Green",
    "4": "Orange",
    "5": "Purple",
    "6": "Pink"
  },
  "passingThreshold": 20,
  "startDate": 1654041600000,
  "endDate": 1654560000000,
  "custodian": "DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17",
  "nftWeight": true,
  "electorate": true
}
```
This 20% minimum majority of the electorate proposal begins on June 1st, 2022 at 00:00 UTC and ends June 7th, 2022 at 00:00 UTC. Since Kusama blocks occur every 6 seconds:

- The first block whose timestamp exceeds or equals the startDate is the inclusive start block of the proposal.
- The last block whose timestamp precedes the endDate is the inclusive end block of the proposal.

Since the snapshot was omitted, the snapshot will be taken at the endDate by default.

Let's assume this was submitted with the appropriate balance transfer at block 12920000. Here's is what the consolidated entity would look like:

### Consolidated
```json
{
  "id": "1e6ttkjfvv",
  "block": 12920000,
  "name": "What is your favorite color?",
  "description": "We want to know our holders’ favorite colors.",
  "collections": ["3208723ec6f65df810-SHELF"],
  "options": {
    "0": "Red",
    "1": "Blue",
    "2": "Yellow",
    "3": "Green",
    "4": "Orange",
    "5": "Purple",
    "6": "Pink"
  },
  "passingThreshold": 20,
  "startDate": 1654041600000,
  "snapshot": 1654560000000,
  "endDate": 1654560000000,
  "custodian": "DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17",
  "nftWeight": true,
  "electorate": true,
  "owner": "DhvRNnnsyykGpmaa9GMjK9H4DeeQojd5V5qCTWd1GoYwnTc",
  "voteFee": "50000000000"
}
```

Note that the `voteFee` is consolidated to ensure voters are guaranteed to only pay the custodian's fee at the time the proposal was submitted.
