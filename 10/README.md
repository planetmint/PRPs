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
* ['metadta']['txids']

The inputs are comprised and written to the data-input of the zenroom execution. Data-input is then defined as
* ['data']['assets']
* ['data']['data']
* ['data']['txids']

The result is written to
* ['metadata']['results']
  
the contract is supposed to write the result into ['metadata']['results'] and the caller has to verfiy if the outcomse is as expected.

### Inputs

#### Transaction Ids


### Result - contract output

