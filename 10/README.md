```
shortname: PRP-10
name: Stateful Smart Contracts
type: Draft
status: Raw
editor: Jürgen Eckel <juergen@riddleandcode.com>, Thomas Fürstner <tom@riddleandcode.com>
```

## Abstract
Planetmint integrates Zenroom smart contracts via the integration of zenroom into [cryptoconditions](https://github.com/planetmint/cryptoconditions).
At this point in time, bitcoin-like smart contracts are supported. These contracts lack the possibilities of internal states. That implies that the conditions and the statements that fulfill them need to be transacted within the very same transaction.
The propsoal of this PRP is to extend the contracts to support states. This is expected to be done in such a way that the states can be changed with new transactions.


## Motivation
Smart contract need to support state in order to enable asynchronouse and party-independent processing of contracts.

## Specification
The inputs for zenroom into the cryptoconditions are defined as follows:

* ['assets']['data']
* ['metadata']['data']
* ['metadata']['txids']

The result is written to
* ['metadata']['results']
  
the contract is supposed to write the result into ['metadata']['results'] and the caller has to verfiy if the outcomse is as expected. The caller does so with the help of the fulfillment-script.

## Input

The ['assets']['data'] and ['metadata']['data'] are JSON data blobs.
The ['metadata']['txids'] is expected to contain an array  of transaction-ids that looks like follows:
``` 
{
    "metadata": {
        "txids": [
            { "a5e52be098746836fee53399878a2cc4ffff1b9edaead7365b7062e2529e61d2" },
            { "8f0ba09f93656f7f2f371bfb098f1096d33f674de9b04f625b47f87196acbd14" }
        ]
    }
}
```

### Preperation
The planetmint node prepares the inputs in such a way that the incoming data is mapped to the following data blobs as follows:

#### Data mappings

* ['data']['assets']        <- ['assets']['data']
* ['data']['metadata']      <- ['metadata']['data']
* ['data']['txids']         <- ['metadata']['txids']

#### Transaction IDs

The incoming transaction IDs are resolved by the ledger and the corresponding raw data is append to the ['data']['txids'] object as follows:
``` 
{
    "data": {
        "txids": [
            { "a5e52be098746836fee53399878a2cc4ffff1b9edaead7365b7062e2529e61d2": { "... raw transaction details" } },
            { "8f0ba09f93656f7f2f371bfb098f1096d33f674de9b04f625b47f87196acbd14": { "... raw transaction details" } }
        ]
    }
}
```

### Result - contract output


### States of Smart Contracts
