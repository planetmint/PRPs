```
shortname: PRP-13
name: Zenroom calling convention
type: Draft
status: Raw
editor: Jürgen Eckel <juergen@riddleandcode.com>
```

## Abstract
Planetmint integrates Zenroom smart contracts via the integration of zenroom into [cryptoconditions](https://github.com/planetmint/cryptoconditions).
The concept of Smart Policies can be derived and integrated with zenroom and the cryptoncidtiona and the transaction schema.


## Motivation
Smart contract are a very powerful concept. At the same time, you need to explicitly interact with them. An explicit defintion of input an output parameteres as well as reserved keywords is needed to enable smooth intgraitons and common understanding. 

## Specification
Planetmint adapt the concept of cryptoconditions. Zenroom based execution on cryptoconditions is defined as follows:
```
zenSha = ZenroomSha256(script=fulfill_script, keys=zen_public_keys, data=data)
```

### Validation & Verification
A zenroom script/contract/policy is verified via a certain input:
```
assert zenSha.validate(message=message)
```
and verifies if this input is a valid input for the above mentioned fulfill script of the ZenroomSha256 call.

### Signing
Signing of a contarct is done by calling
```
zenSha.sign(script_input, condition_script, alice)
```
This will execute the conditional script together with the private keys of alice and the script input. 

Usually, the validation script can verify if the conditional script executed as requested by passing the output data (e.g. signatures) from the signing process to the validate call. 

### Inputs & Outputs
The zenroom/zencode integration of Planetmint defines the following calling input/output convention for the contracts/policies/scritps.

The input messages (e.g. the script inputs) are expected to be structured as follows:
```
{
    "input" : {

    },
    "output" : {

    }
    "signature": {

    }
}
```
The "input" contains all the data that is to be passed to the script being exectued (fulfilled, verified, ...).
The "output" contains all the output data of the script/contract/policy execution including the exectuion logs.
The "signature" is a special case output value. This value is automatically merged into the script input if it is passed from a message signing call into the validate method.

The major tags: "input", "output", and "signature" are therefore predefined tags that must not be used otherwise.

