```
shortname: PRP-10
name: Stateful Smart Contracts
type: Draft
status: Raw
editor: Jürgen Eckel <juergen@riddleandcode.com>, Thomas Fürstner <tom@riddleandcode.com>, Lorenz Herzberger <lorenzherzberger@gmail.com>
```

## Abstract
Planetmint integrates Zenroom smart contracts via the integration of zenroom into [cryptoconditions](https://github.com/planetmint/cryptoconditions).
At this point in time, bitcoin-like smart contracts are supported. These contracts lack the possibilities of internal states. That implies that the conditions and the statements that fulfill them need to be transacted within the very same transaction.
The propsoal of this PRP is to extend the contracts to support states. This is expected to be done in such a way that the states can be changed with new transactions.


## Motivation
Smart contract need to support state in order to enable asynchronous and party-independent processing of contracts.

## Specification
A new optional `script` tag is added to the transaction specification:

It is defined as follows:
```json
{
  "script": {
    "code": {
      "type": "string",
      "raw": "string",
      "parameters": "object", // or
      "tx_id": "string"
    },
    "state": "string || object",
    "input": "object",
    "output": "object",
    "policies": "object"
  }
}
```

The contract is supposed to write the result into ['script']['output'] and the caller has to verfiy if the outcome is as expected. The caller does so with the help of the fulfillment-script.

For a more detailed explaination please refer to the transaction specification in [PRP-9](../9)

## Input
The ['script']['input'] is an arbitrary JSON data blobs. 

### Preperation
The planetmint node prepares the inputs in such a way that the incoming data is mapped to the following data blobs as follows:

#### Data mappings
* ['data']['input']        <- ['script']['input']

### Result - contract output
The contract output and result is written to ['script']['output'] and depends completley on the the contract itself. The caller therefore has to verify the result and the outcome. For a stateful smart contract the output hold the state of the contract after the state transition.  

### States of Smart Contracts
The state of the smart contract will be written to ['script']['output']. The state can then be referenced with the ['script']['state'] which either represents the initial state of the contract or the latest transaction that holds the current state.

### Contract Creation
The creation of a contract is very simple. A `CREATE` transaction must be called defining all the parameters for the contract. A creator of a contract must provide the ['code']['type'] (e.g.: Zenroom, Solidity, etc.), the actual ['code']]['raw'] and the input ['code']['parameters'] for the contract.

#### Reusealbe contracts - state-ful contracts
A contract can persist states after being created. This can be achieved by the `TRANSFER` transaction. This type of transaction enables the contract to persist data in the result-output in such a way that it can be reutilized in the next iteration/exeuction. Therewith, states can be referenced without the need to extend the framework or the integration. It's more a matter of a convention like on memory stacks. Handover-routines need to be defined to make it transparent.
