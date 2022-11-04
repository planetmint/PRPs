```
shortname: PRP-10
name: Stateful Smart Contracts
type: Draft
status: Raw
editor: Jürgen Eckel <juergen@riddleandcode.com>, Thomas Fürstner <tom@riddleandcode.com>, Lorenz Herzberger <lorenzherzberger@gmail.com>
```

# Abstract
Planetmint integrates Zenroom smart contracts via the integration of zenroom into [cryptoconditions](https://github.com/planetmint/cryptoconditions).
At this point in time, bitcoin-like smart contracts are supported. These contracts lack the possibilities of internal states. That implies that the conditions and the statements that fulfill them need to be transacted within the very same transaction.
The propsoal of this PRP is to extend the contracts to support states. This is expected to be done in such a way that the states can be changed with new transactions.


# Motivation
Smart contract need to support state in order to enable asynchronous and party-independent processing of contracts.

# Specification
A new optional `script` tag is added to the transaction specification:

It is defined as follows:
```json
{
  "script": 
  {    "code": {
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

The contract is supposed to write the result into ['script']['output'] and the caller has to verfiy if the outcome is as expected. The caller can verify the result with the fulfillment-script.

For a more detailed explaination please refer to the transaction specification in [PRP-9](../9)

# Input
The ['script']['input'] is an arbitrary JSON data blob. 

## Preperation
The planetmint node prepares the inputs in such a way that the incoming data is mapped to the following data blobs as follows:

## Data mappings
* ['data']['input']        <- ['script']['input']

## Result - contract output
The contract output and result is written to ['script']['output'] and depends completley on the the contract itself. The caller therefore has to verify the result and the outcome. For a stateful smart contract the output hold the state of the contract after the state transition.  

## States of Smart Contracts
The state of the smart contract will be written to ['script']['output']. The state can then be referenced with the ['script']['state'] which either represents the initial state of the contract or the latest transaction that holds the current state.

## Contract/Script Creation
The following paragraphs will walk you through the creation of a zenroom contract/script on planetmint. The code that this is derived from can be found in the [acceptance tests](https://github.com/planetmint/planetmint/blob/main/acceptance/python/src/test_zenroom.py) of planetmint.

The creation of a contract/script is very simple. 

The script is encoded into a cryptocondition and contains the conditions that need to be fulfilled (e.g. FULFILL_SCRIPT).

```python
zenroomscpt = ZenroomSha256(
                script=FULFILL_SCRIPT,
                data=INITIAL_STATE,
                keys=zen_public_keys)
```
The condition will be encoded together with the initial state and public keys and is added to the transaction via an ```condition``` in the inputs or outputs.

```python
    condition_uri_zen = zenroomscpt.condition.serialize_uri()
    unsigned_fulfillment_dict_zen = {
        "type": zenroomscpt.TYPE_NAME,
        "public_key": base58.b58encode(signer.public_key).decode(),
    }
    output = {
        "amount": "10",
        "condition": {
            "details": unsigned_fulfillment_dict_zen,
            "uri": condition_uri_zen,
        },
        "public_keys": [
            signer.public_key,
        ],
    }
```

The script tag can then be used to define inputs and expected outputs
```json
    script_ = {
        "code": {"type": "zenroom", "raw": "test_string", "parameters": [{"obj": "1"}, {"obj": "2"}]},  # obsolete
        "state": "dd8bbd234f9869cab4cc0b84aa660e9b5ef0664559b8375804ee8dce75b10576",
        "input": SCRIPT_INPUT,
        "output": ["ok"],
        "policies": {},
    }
```

The script tag and the script to fulfill the conditions is then added to the initial script and signed by the actor (in this case alice):

```
  signed_input = zenroomscpt.sign(script_, CONDITION_SCRIPT, alice)
```

The state of the script can then be verified by executing the initial ```FULFILL_SCRIPT```. Therefore, the output of the ```CONDITION_SCRIPT``` execution in combination with ```script_``` needs to be mapped to the input and some output needs to be removed first:

```
  input_signed = json.loads(signed_input)
  input_signed["input"]["signature"] = input_signed["output"]["signature"]
  del input_signed["output"]["signature"]
  del input_signed["output"]["logs"]
```
This can now be verified by executing the scripts locally
```
  input_msg = json.dumps(input_signed)
  zenroomscpt.validate(message=input_msg)
```

or by adding the context of the script executing to the transaction, singing it and sending it to planetmint

```
  # add the fulfillment and the signed input
  tx["inputs"][0]["fulfillment"] = fulfillment_uri_zen
  tx["script"] = input_signed
  tx["id"] = None

  # convert the json string
  json_str_tx = json.dumps(tx, sort_keys=True, skipkeys=False, separators=(",", ":"))

  # SHA3: hash the serialized id-less transaction to generate the id
  shared_creation_txid = sha3_256(json_str_tx.encode()).hexdigest()
  tx["id"] = shared_creation_txid
  
  # send the signed TX to planetmint
  plntmnt = Planetmint(os.environ.get("PLANETMINT_ENDPOINT"))
  sent_transfer_tx = plntmnt.transactions.send_commit(tx)
```


### Reusealbe contracts - state-ful contracts
A contract can persist states after being created. This can be achieved by the `TRANSFER` transaction. This type of transaction enables the contract to persist data in the result-output in such a way that it can be reutilized in the next iteration/exeuction. Therewith, states can be referenced without the need to extend the framework or the integration. It's more a matter of a convention like on memory stacks. Handover-routines need to be defined to make it transparent.
