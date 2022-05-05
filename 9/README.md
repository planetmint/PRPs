```
shortname: PRP-9
name: Planetmint Transactions Spec v3
type: Standard
status: Draft
editor: Lorenz Herzberger <lorenzherzberger@gmail.com>
```

# Abstract

This PRP contains a specification of a Planetmint transaction, including:
 - The pieces of a Planetmint Transaction
 - Explanations of how one can assemble or compute the pieces
 - What it means for a Planetmint transaction to be "valid"  and to check for validity
 - Explanations of what has changed from v2

# Motivation

As Planetmint evolves the need for multi-asset support arose. Mainly in the form of the new transaction types mentioned in [PRP-5 New types of operations for the transactional model]. In the BigchainDB Transaction Spec v2 only a single asset is allowed per transaction which in turn does not allow for inputs from more than one previous transaction. The new transaction types COMPOSE and DECOMPOSE need different assets hence the need for an updated transaction specification. 

This should also serve as the first version of the Transaction Spec after the transition from BigchainDB to Planetmint.

# Changes between Version 2 and 3 of this Spec
## 1. Allowed "version" value changes from "2.0" to "3.0"
In v2.0, the only allowed value for the key "version" was "2.0". In v3.0, the only allowed value for the key "version" is "3.0".

## 2. Changed asset to assets

## 3. New transactional types
In v3.0 of the transaction spec the set of allowed transaction types is enlarged by `COMPOSE` and `DECOMPOSE`. These new transaction types have their respective validation rules.