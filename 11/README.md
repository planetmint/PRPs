```
shortname: PRP-11
name: Smart Policies
type: Draft
status: Raw
editor: Jürgen Eckel <juergen@riddleandcode.com>, Thomas Fürstner <tom@riddleandcode.com>
```

## Abstract
Planetmint integrates Zenroom smart contracts via the integration of zenroom into [cryptoconditions](https://github.com/planetmint/cryptoconditions).
The concept of Smart Policies can be derived and integrated with zenroom and the cryptoncidtiona and the transaction schema.


## Motivation
Smart contract are a very powerful concept. At the same time, you need to explicitly interact with them. Network-wide or asset-based policies cannot be enfored with contracts alone. A concept to enforce policies on assets or certain types of transactions needs is missing and needs to be defined.

## Specification
Zenroom based smart contracts can be defined and are precisely specified at [PRP-10](../10).
Asset based smart policies are smart contracts that verify integrity and garantee compliance towards a given contract.
The smart policy is a contract verifying the transaction state before it is finally commited to the network.

The smart policy has to be mentioned as an asset based reference in
1. ['script']['policies']['txids'] and will be resolved by the DLT nodes, or
2. can be defined in ['script']['policies']['raw']['0', ...]


## Input

['script']['policies']['txids'] is expected to contain an array  of transaction-ids that looks like follows:
``` 
{
    "policies": {
        "txids": [
            { "a5e52be098746836fee53399878a2cc4ffff1b9edaead7365b7062e2529e61d2" },
            { "8f0ba09f93656f7f2f371bfb098f1096d33f674de9b04f625b47f87196acbd14" }
        ]
    }
}
```
another option is to list the policies within an array beginning with :
``` 
{
    "policies": {
        "raw": [
            { "0" : "raw zenroom policy 1" },
            { "1" : "raw zenroom policy 2" }
        ]
    }
}
```
### Preperation
The planetmint node prepares the inputs in such a way that the incoming data is mapped to the following data blobs as follows:


#### Transaction IDs

The incoming transaction IDs are resolved by the ledger and the corresponding raw data is appended to the ['policies']['txids'] object as follows:
``` 
{
    "policies": {
        "txids": [
            { "a5e52be098746836fee53399878a2cc4ffff1b9edaead7365b7062e2529e61d2": { "... raw transaction details" } },
            { "8f0ba09f93656f7f2f371bfb098f1096d33f674de9b04f625b47f87196acbd14": { "... raw transaction details" } }
        ],
        "raw": [
            { "0" : "raw zenroom policy 1" },
            { "1" : "raw zenroom policy 2" }
        ]
    }
}
```

### Result - contract output

The policy output and result is written to ['script']['output']['policies']['txids']['$txid'] and ['script']['output']['policies']['raw']['$raw_id'] and depends completely on the the policy itself. The caller therefore has to verify the result and the outcome.
