# Result

A result entity is the certified vote count of a proposal from a custodian, whether they were assigned the proposal or they were assigned to
[RECERTIFY](../interactions/RECERTIFY.md). The count object keys should equal the proposal's options' keys. Any omitted keys will default to a count of 0 and any duplicate keys in the count object will default to the last duplicate key's count. Any keys that do not match any of the proposal's options' keys will be disregarded.

```json
{
  "proposalId": {
    "type": "string",
    "value": "the proposal id"
  },
  "count": {
    "type": "object",
    "value": {
      "0": {
        "type": "number",
        "value": "the count for option 1 given the parameters of the proposal. Note, the keys to the count values must equal the keys of the proposal options. Any duplicate keys will cause JSON.parse() to default to the last duplicate key-value pair in the object."
      },
      "1": {
        "type": "number",
        "value": "the count for option 2 given the parameters of the proposal."
      },
      "2": {
        "type": "number",
        "value": "the count for option 3 given the parameters of the proposal."
      },
      "...": "...",
      "n-1": {
        "type": "number",
        "value": "the count for option n given the parameters of the proposal."
      }
    }
  },
  "winningOptions": {
    "type": "array",
    "value": "the option(s) with the most votes. If none of the options meet the passingThreshold, this should be an empty array. More than one value implies a tie."
  },
  "thresholdDenominator": {
    "type": "number",
    "value": "if electorate is false for the proposal, this is simply the sum of all options' counts. If electorate is true and the proposal's nftWeight is true, this value should be the number unburned NFTs in all of the collections in the proposal's collections array. If electorate is true and nftWeight is false, this value should be the number of unique NFT holders across all of the collections in the proposal's collecitons array."
  },
  "recertify": {
    "type": "boolean",
    "value": "if false, this result is being submitted because the custodian was originally assigned to the proposal. If true, the custodian was assigned to recertify the proposal."
  }
}
```
## Computed fields

To distinguish the possibility that a proposal is recertified by another custodian or if a custodian resubmits results, a RESULT entity shall have a computed id
as follows:

`{proposalId}-{custodianKusamaAddress}-{resultBlockNumber}`

For example, `1e6ttkjfvv-DaoQkNgxobgpKfd9NmETnWe6FCfDERduSTR2YiFDRGKvL17-14300000`

## Example

Let's say [this proposal example](proposal.md#example) has been completed. The results might look something like this:

```json
{
  "proposalId": "1e6ttkjfvv",
  "count": {
    "0": 50.5,
    "1": 200.25,
    "2": 25,
    "3": 150.25,
    "4": 10.5,
    "5": 300,
    "6": 700.25
  },
  "winningOptions": [6],
  "thresholdDenominator": 1436.75,
  "recertify": false
}
```
