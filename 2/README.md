```
shortname: PRP-2
name: Consensus-Oriented Specification System
type: Meta
status: Draft
editor: Jürgen Eckel <juergen@riddleandcode.com>
contributors: JÜrgen Eckel <juergen@riddleandcode.com>
```

This document describes a consensus-oriented specification system (COSS) for building interoperable technical specifications. COSS is based on a lightweight editorial process that seeks to engage the widest possible range of interested parties and move rapidly to consensus through working code.

This specification is based on [unprotocols.org 2/COSS](https://rfc.unprotocols.org/2/) and on [EIP1 - EIP Purpose and Guidelines](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1.md).

## Change Process
This document is governed by the [PRP-2 (COSS)](../2/README.md).

## Language
The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [BCP 14](https://tools.ietf.org/html/bcp14) \[[RFC2119](https://tools.ietf.org/html/rfc2119)\] \[[RFC8174](https://tools.ietf.org/html/rfc8174)\] when, and only when, they appear in all capitals, as shown here.

## Goals
The primary goal of COSS is to facilitate the process of writing, proving, and improving new technical specifications. A "technical specification" defines a protocol, a process, an API, a use of language, a methodology, or any other aspect of a technical environment that can usefully be documented for the purposes of technical or social interoperability.

A Planetmint specification is called **P**lanetmint **R**efinement **P**roposal, **PRP** henceforth.

COSS is intended to above all be economical and rapid, so that it is useful to small teams with little time to spend on more formal processes.

Principles:

* We aim for rough consensus and running code.
* PRPs are small pieces, made by small teams.
* PRPs should have a clearly responsible editor.
* The process should be visible, objective, and accessible to anyone.
* The process should clearly separate experiments from solutions.
* The process should allow deprecation of old PRPs.

PRPs should take minutes to explain, hours to design, days to write, weeks to prove, months to become mature, and years to replace.

PRPs have no special status except that accorded by the community.

The author of the PRP is responsible for building consensus within the community and documenting dissenting opinions.

## Architecture

### Types of PRP
There are three types of PRPs:
* A **Standard Track PRP** describes any change to network protocols, transaction validity rules, proposed application standards/conventions, or any change or addition that affects the interoperability of applications using BigchainDB products.
* An **Informational PRP** describes a BigchainDB design issue, or provides general guidelines or information to the BigchainDB community, but does not propose a new feature. Informational PRPs do not necessarily represent BigchainDB community consensus or a recommendation, so users and implementers are free to ignore Informational PRPs or follow their advice.
* A **Meta PRP** describes a process surrounding BigchainDB or proposes a change to a process.

### PRP Format
A PRP is a set of Markdown documents (the main file SHOULD be called `README.md`), together with comments, attached files, and other resources. A PRP is identified by its number (e.g. this PRP is **PRP-2**). The number of the PRP is also the name of the directory where its files are stored.

Every PRP (including branches) carries a different number. New versions of the same PRP have new numbers.

### PRP template
Each PRP MUST customize and include this header:
````
```
shortname: [number/shortname]
name: [Full name of the BEP]
type: [standard | informational | meta ]
status: [raw | draft | stable | deprecated | retired | deleted]
editor: [Editor Name <email address>]
contributors: [Optional Contributor 1 <email address>, ..., Optional Contributor N <email address>]
```
````
_Note: the `number` is assigned after a PRP has been submitted._

Each PRP SHOULD include the following sections:

1. **Abstract**. The abstract is a short (~200 word) informal description of the technical issue being addressed.

1. **Motivation**. The motivation is a possibly long informal description of the issue being addressed. The motivation is critical for PRPs that want to change the BigchainDB protocol. It should clearly explain why the existing protocol is inadequate to address the problem that the PRP solves. PRP submissions without sufficient motivation may be rejected outright.

1. **Problem Breakdown** The detailed formal description of the problem in the form of the list of exact issues the solution has to address.

1. **Specification**: The technical specification should describe the syntax and semantics of any new feature. The specification must address the exact issues described in the solution breakdown and should describe how it addresses them. The specification should be detailed enough to allow competing, interoperable implementations. It MAY describe the impact on data models, API endpoints, security, performance, end users, deployment, documentation, and testing.

1. **Rationale**. The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.

1. **Backwards Compatibility**. All PRPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The PRP must explain how the author proposes to deal with these incompatibilities. PRP submissions without a sufficient backwards compatibility treatise may be rejected outright.

1. **Implementation**. The implementations must be completed before any PRP is given status "stable", but it need not be completed before the PRP is accepted. While there is merit to the approach of reaching consensus on the PRP and rationale before writing code, the principle of "rough consensus and running code" is still useful when it comes to resolving many discussions of API details.

1. **Copyright Waiver**. Except for PRP-1 (C4) and PRP-2 (COSS), all PRPs MUST be released to the public domain. To do that, the following block of HTML SHOULD be used in the BEP's source Markdown file. ([HTML is valid Markdown](https://daringfireball.net/projects/markdown/syntax#html).)

```html
<p xmlns:dct="http://purl.org/dc/terms/">
  <a rel="license"
     href="http://creativecommons.org/publicdomain/zero/1.0/">
    <img src="http://i.creativecommons.org/p/zero/1.0/88x31.png" style="border-style: none;" alt="CC0" />
  </a>
  <br />
  To the extent possible under law, all contributors to this BEP
  have waived all copyright and related or neighboring rights to this PRP.
</p>
```

The first 4 sections above are based on the [Leslie Lamport's note about describing solutions](https://lamport.azurewebsites.net/pubs/state-the-problem.pdf).

## COSS Lifecycle
Every PRP has an independent lifecycle that documents clearly its current status.

A PRP has six possible states that reflect its maturity and contractual weight:

![Lifecycle diagram](lifecycle.png)

### Raw PRPs
All new PRPs are **raw** PRPs. Changes to raw PRPs can be unilateral and arbitrary. Those seeking to implement a raw PRP should ask for it to be made a draft PRP. Raw PRPs have no contractual weight.

### Draft PRPs
When raw PRPs can be demonstrated, they become **draft** PRPs. Changes to draft PRPs should be done in consultation with users. Draft PRPs are contracts between the editors and implementers.

### Stable PRPs
When draft PRPs are used by third parties, they become **stable** PRPs. Changes to stable PRPs should be restricted to cosmetic ones, errata and clarifications. Stable PRPs are contracts between editors, implementers, and end-users.

### Deprecated PRPs
When stable PRPs are replaced by newer draft PRPs, they become **deprecated** PRPs. Deprecated PRPs should not be changed except to indicate their replacements, if any. Deprecated PRPs are contracts between editors, implementers and end-users.

### Retired PRPs
When deprecated PRPs are no longer used in products, they become **retired** PRPs. Retired PRPs are part of the historical record. They should not be changed except to indicate their replacements, if any. Retired PRPs have no contractual weight.

### Deleted PRPs
Deleted PRPs are those that have not reached maturity (stable) and were discarded. They should not be used and are only kept for their historical
value. Only Raw and Draft PRPs can be deleted.

## Editorial control
A PRP MUST have a single responsible editor, the only person
who SHALL change the status of the PRP through the lifecycle stages.

A PRP MAY also have additional contributors who contribute changes to it. It is RECOMMENDED to use the [C4 process](../1/README.md) to maximize the scale and diversity of contributions.

The editor is responsible for accurately maintaining the state of PRPs and for handling all comments on the PRP.

## Branching and Merging
Any member of the domain MAY branch a PRP at any point. This is done by copying the existing text, and creating a new PRP with the same name and content, but a new number. The ability to branch a PRP is necessary in these circumstances:

* To change the responsible editor for a PRP, with or without the cooperation of the current responsible editor.
* To rejuvenate a PRP that is stable but needs functional changes. This is the proper way to make a new version of a PRP that is in stable or deprecated status.
* To resolve disputes between different technical opinions.

The responsible editor of a branched PRP is the person who makes the branch.

Branches, including added contributions, SHOULD be dedicated to the public domain using CC0 (just like the original BEP). This means that contributors are guaranteed the right to merge changes made in branches back into their original PRPs.

Technically speaking, a branch is a *different* PRP, even if it carries the same name. Branches have no special status except that accorded by the community.

## Conflict resolution
COSS resolves natural conflicts between teams and vendors by allowing anyone to define a new PRP. There is no editorial control process except that practised by the editor of a new PRP. The administrators of a domain (moderators) may choose to interfere in editorial conflicts, and may suspend or ban individuals for behaviour they consider inappropriate.

## Conventions
Where possible editors and contributors are encouraged to:

* Refer to and build on existing work when possible, especially IETF specifications.
* Contribute to existing PRPs rather than reinvent their own.
* Use collaborative branching and merging as a tool for experimentation.

## License
Copyright (c) 2008-16 Yurii Rashkovskii <yrashk@gmail.com>, Pieter Hintjens <ph@imatix.com>, André Rebentisch <andre@openstandards.de>, Alberto Barrionuevo <abarrio@opentia.es>, Chris Puttick <chris.puttick@thehumanjourney.net>
Copyright (c) 2022 IPDB Foundation e.V.

This PRP is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 3 of the License, or (at your option) any later version.

This PRP is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program; if not, see http://www.gnu.org/licenses.
