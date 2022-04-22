```
shortname: PRP-8
name: Adding tradable asset attribution - attribution/material balance
type: Standard
status: Raw
editor: Jürgen Eckel <juergen@riddleandcode.com>, Thomas Fürstner <tom@riddleandcode.com>
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

One major of aspect of the integration is that material and attribute balance being notarized is respected within the scope of an asset product or processing batch. That means that all notarized attributions have to be passed furhter in the supply chain and cannot be withhold.
The relation and distribution of attributions to each and every element of the processing/production batch is not defined by this proposal. Instead, this is delegated further to those using the attribution/material balance framework to optimize their processes and opportunities. 

This PRP exists to define the basic requirements for the integration within Planetmint.

## Specification
Material gets processed and is transformed. One simple scenario of this is the transformation of 1 asset calsse into another asset class (e.g. slicing a tree trunk, transforming steel bars into screws, ...)
* asset type A (amount 1, carbon footprint 1000 units)) 
is processed and results in
* asset type B (amount 1000).
 
Imagine A comes with carbon footprint of 1000 units. This means that B has to have at least carbon footprint of 1000 units. 
The transformation process can of course come with carbon emissions, too. That would mean that B is comprised of the carbon footprint of asset Aand the carbon footprint of the transformation process C. With both having e.g. a 1000 unit carbon footprint. This will result in a carbon footprint of 2000 units for B.
* Transformation process (carbon footprint 1000 units)

After the transformation process 1000 units of B with an overall carbon footproint of 2000 unit exits. The 1000 units of B can be sold seperately. So can the 2000 units of carbon emssions. They don't have to be associated with the each and every elemtn of the 1000 unit batch, but need to be sold and during the process of selling the batch of the 1000 units of B.

This can come in various possibilities:
* selling 999 units of B without any units of carbon emssion and 1 B with 2000 units of carbon emission.
* selling each unit of B with 2 units of carbon emission

The process of selling the carbon emission is aligned to the overall process of selling the production batch, but can be optimized to not affect each and every unit of B. This leaves room for business model improvements and optimiations, and takes care about respecting all carbon emissions being notarized.
This is the definition of the attribution/material balance.


