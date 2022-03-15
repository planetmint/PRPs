```
shortname: PRP-5
name: New types of operations for the transactional modell
type: Meta
status: Draft
editor: Jürgen Eckel <juergen@riddleandcode.com> 
contributors: 
```

# Note

# Abstract

This PRP is about COMPOSE and DECOMPOSE transactions.

# Motivation

Industrial processes bring together different components. This happen in different types of scale, e.g. on atomic level, within the domain of metal components, platic, and others. That's what is refered to as composition process.
The goal of recycling procsses is the segregation of the products, a decomposition process.

The proposed transaction types are expected to just model these to processes: COMPOSE and DECOMPOSE.

# Specification

The current transactional modell is to be extended about two operations
* COMPOSE
* DECOMPOSE

## COMPOSE
It is assumed that Alice has 4 different components A, B, C, D with amount 20, 15, 10, 5.
Alice creates a new product Z out of the 4 components, and needs 5A, 5B, 5C and 1D to create 1 Z.

Alice also uses machines to create Z that operate on a high frequence. A batch operation is need to actually realize a meaningful and real-world related operational model. 
The proposal is to define arbitary number of inputs for the operation with arbitrary amounts e.g. [ 20 A, 15 B, 10 C 5 D].

The amount in the operation reflects the size of batch that is being created from the inputs e.g. 2.

This will result in the 
* creation of 2 Z (using [10 A, 10 B, 10 C and 2 D]) and thereby burning
* the followoing assets [10 A, 10 B, 10 C and 2 D] MUST NOT be assigned to any output

Alice will thereafter hold [5 A, 5 B, 0 C, 3 D, 2 Z]
It is assumed that Z has asset id: 41c6cd47fd316ef9e7c86640c5004296f8d384800445da956f45ec835140384e

## DECOMPOSE

Bob retrieves 1 Z with asset id 41c6cd47fd316ef9e7c86640c5004296f8d384800445da956f45ec835140384e.
Bob wants to recylce Z and decomposes Z into [5A, 5B, 5C, 1 D].

This will result in the 
* the followoing asset Z is not assigned to any output 
* and the creation of [5 A, 5 B, 5 C and 1 D]

**NOTE that the decomposition process can result in fewer components that the original component got created with. 
This is due to waste/abuse or quality constraints. There is therefore on condition that says that number of elements during the DECOMPOSITION operation can only be maximum the number of elements being consumed/used during the COMPOSITION operation. The difference is to be considered as waste.

# Implementation

Burning an asset is done by sending it to the following address: 
'BurnBurnBurnBurnBurnBurnBurnBurnBurnBurnBurn'
that is 11 times 'Burn'. Burning assets is an implicit transfer transaction and difficult to modell without getting this transfer explicitly signed by the signing party.
Instead, an implicit burning of the asset is proposed. The implicit burning is done by not locking the asset by an output - not assigning and input to any output.
This means that the amount of assets being utilized to create the asset Z (COMPOSE) is taken as an input, but not forwarded to any output.
The asset is not assigned to anyone and thereby non existant. It's a burning by non-assignment. 

## Verification
The verification of the transactionshould look like follows:
* verify inputs
* verify consumption of inputs: that each output asset that is also input has at less amount than before.
* verify that something got created, a new asset type, that does not have a corresponding input



## Integration
The operation is expected to be atomic and needs to be integrated into 
* planetmint
* planetmint-python


# Credits
The Developer Certificate of Origin was developed by the Linux community and has since been adopted by other projects, including many under the Linux Foundation umbrella (e.g. Hyperledger Fabric). The process described above (with the Signed-off-by line in Git commits) is also based on [the process used by the Linux community](https://github.com/torvalds/linux/blob/master/Documentation/process/submitting-patches.rst#11-sign-your-work---the-developers-certificate-of-origin).

# Copyright Waiver

<p xmlns:dct="http://purl.org/dc/terms/">
  <a rel="license"
     href="http://creativecommons.org/publicdomain/zero/1.0/">
    <img src="http://i.creativecommons.org/p/zero/1.0/88x31.png" style="border-style: none;" alt="CC0" />
  </a>
  <br />
  To the extent possible under law, all contributors to this BEP
  have waived all copyright and related or neighboring rights to this PRP.
</p>