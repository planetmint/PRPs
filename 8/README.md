```
shortname: PRP-8
name: Adding tradable asset attribution - attribution/material balance
type: Standard
status: Raw
editor: Jürgen Eckel <juergen@riddleandcode.com>, Thomas Fürstner <tom@riddleandcode.com>
contributors: Lorenz Herzberger <lorenzherzberger@gmail.com>
```

## Abstract
Planetmint is able to modell fungable and non-fungable tokens and assets. The basic principle behind all asset groups is the amount of assets, their asset description (data) and metadata of the asset. 
The goal of this PRP is to enhance this concept about arbitrary additional attributions that can be handled by planetmint and the transaction modell.
This is called attribution/material balance.


## Motivation
Connecting industrial processes comes with the demand to be compliant to regulation. Therefore regulatory concepts need to be reflected in order to support the industrial adaption of DLT.

So far, the following set of attributions have been identified
1. carbon emissions
2. hazardous material (not addressed in this document)
3. degree and various types of recycled material (not addressed in this document)
4. composition of asset classes

One major of aspect of the integration is that material and attribute balance being notarized is respected within the scope of an asset product or processing batch. That means that all notarized attributions have to be passed furhter in the supply chain and cannot be withhold.
The relation and distribution of attributions to each and every element of the processing/production batch is not defined by this proposal. Instead, this is delegated further to those using the attribution/material balance framework to optimize their processes and opportunities. 

This PRP exists to define the basic requirements for the integration within Planetmint.

## Specification
Material gets processed and is transformed. One simple scenario of this is the transformation of 1 asset class into another asset class (e.g. slicing a tree trunk, transforming steel bars into screws, ...)
* asset type A (amount 1, carbon footprint 1000 units)) 
is processed and results in
* asset type B (amount 1000).
 
Imagine A comes with carbon footprint of 1000 units. This means that B has to have at least carbon footprint of 1000 units. 
The transformation process can of course come with carbon emissions, too. That would mean that B is comprised of the carbon footprint of asset A and the carbon footprint of the transformation process C. With both having e.g. a 1000 unit carbon footprint. This will result in a carbon footprint of 2000 units for B.
* Transformation process (carbon footprint 1000 units)

After the transformation process 1000 units of B with an overall carbon footproint of 2000 unit exits. The 1000 units of B can be sold seperately. So can the 2000 units of carbon emssions. They don't have to be associated with the each and every element of the 1000 unit batch, but need to be sold and during the process of selling the batch of the 1000 units of B.

This can come in various possibilities:
* selling 999 units of B without any units of carbon emssion and 1 B with 2000 units of carbon emission.
* selling each unit of B with 2 units of carbon emission

The process of selling the carbon emission is aligned to the overall process of selling the production batch, but can be optimized to not affect each and every unit of B. This leaves room for business model improvements and optimiations, and takes care about respecting all carbon emissions being notarized.
This is the definition of the attribution/material balance.

# Implementation
The transaction schema for outputs must be enhanced by the `composite_amount` property.
It indicates the composition of the created, transfered, composed or decomposed assets.
It can be comprised of any number of keys and their respective value as `integer`. If any conversions from `integers` are needed they must be provided.

For a `CREATE` transaction the `composite_amount` simply describes the composition. The only restriction is: `sum of output amounts >= 0`. An example is provided below:

```json
{
  "id": "38100137cea87fb9bd751e2372abb2c73e7d5bcf39d940a5516a324d9c7fb88d",
  "operation": "CREATE",
  "assets": [
    {"data": "Qme2Gc15TFi3XEbU87WT9zXDfAYPJQku8vpUkjFaoZsttQ"}
  ],
  "outputs": [
    {
      "amount": 2400,
      "composite_amount": {
        "carbon_offset": 300,
        "C71": 1500,
        "C72": 50,
        "C73": 10000
      },
      "condition": "producer_output_condition",
      "public_keys": ["producer_public_key"]
    }
  ],
  // ...
}
```

As specified in the [PRP-9](../9/) `TRANSFER` transactions must have at least 1 input. The restriction for `composite_amounts` is the same as for output `amounts` meaning: `sum of input amounts == sum of output amounts`. 

```json
{
  "id": "0e7a9a9047fdf39eb5ead7170ec412c6bffdbe8d7888966584b4014863e03518",
  "operation": "TRANSFER",
  "assets": [
    {
      "id": "38100137cea87fb9bd751e2372abb2c73e7d5bcf39d940a5516a324d9c7fb88d"
    }
  ],
  "outputs": [
    {
      "amount": 2000,
      "composite_amount": {
        "carbon_offset": 250,
        "C71": 1000,
        "C72": 25,
        "C73": 2000
      },
      "condition": "producer_output_condition",
      "public_keys": ["producer_public_key"]
    },
    {
      "amount": 400,
      "composite_amount": {
        "carbon_offset": 50,
        "C71": 500,
        "C72": 25,
        "C73": 8000
      },
      "condition": "buyer_output_condition",
      "public_keys": ["buyer_public_key"]
    }
  ],
  // ...
}
```

`COMPOSE` transactions can consume multiple assets to create a new one. For more information refer to [PRP-5](../5). The same rules as for a `TRANSFER` transactions apply, however it is mandatory to define a new asset alongside the consumed assets.

```json
{
  "id": "a5e52be098746836fee53399878a2cc4ffff1b9edaead7365b7062e2529e61d2",
  "operation": "COMPOSE",
  "assets": [
    {"data": "bafyreie74tgmnxqwojhtumgh5dzfj46gi4mynlfr7dmm7duwzyvnpw7h7m"},
    {"id": "38100137cea87fb9bd751e2372abb2c73e7d5bcf39d940a5516a324d9c7fb88d"},
    {"id": "0e7a9a9047fdf39eb5ead7170ec412c6bffdbe8d7888966584b4014863e03518"}
  ],
  "outputs": [
    {
      "amount": 2000,
      "composite_amount": {
        "carbon_offset": 250,
        "C71": 1000,
        "C72": 25,
        "C73": 2000
      },
      "condition": "producer_output_condition",
      "public_keys": ["producer_public_key"]
    },
    {
      "amount": 400,
      "composite_amount": {
        "carbon_offset": 25,
        "C71": 250,
        "C72": 25,
        "C73": 4000
      },
      "condition": "producer_output_condition",
      "public_keys": ["producer_public_key"]
    },
    {
      "amount": 400,
      "composite_amount": {
        "carbon_offset": 25,
        "C71": 250,
        "C73": 4000
      },
      "condition": "producer_output_condition",
      "public_keys": ["producer_public_key"]
    },
  ]
}
```

`DECOMPOSE` transactions do the same but in reverse consuming a certain composite and creating new assets from it.
