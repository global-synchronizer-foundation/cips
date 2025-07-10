## CIP XXXX

<pre>
 CIP: CIP XXXX
 Title: Bootstrap network from non-zero round
 Author: Julien Tinguely
 Comments-Summary: No comments yet
 Status: Draft
 Type: Standards Track
 Created: 2025-07-09
 Approved: ?
 License: CC0-1.0
</pre>

## Abstract

This CIP proposes to modify the network's bootstrapping process allowing SV nodes to start from a pre-configured non-zero round. 
Currently, on network resets, all SV nodes begin at round zero, which also means they revert to the initial coin issuance curve. 
Our goal is to enable network resets that maintain historical round progression, rather than resetting to zero. 
This approach aims to better align TestNet with MainNet's ongoing state.

## Specification

### Daml

A new optional argument `initialRound` is added to the `DsoRules_Bootstrap` choice.
When provided, this value is used as the initial round in the network so rather than creating round 0, 1 and 2 when initializing 
we create `initialRound`, `initialRound + 1`, `initialRound + 2`. The rounds are created with `issuingFor` set to the same time 
in the issuance curve that would be used if rounds advanced one by one starting from 0.

### SV Application

SV1 will read the new optional flag `SPLICE_APP_SV_INITIAL_ROUND` defaulted to 0 and set it as initial round.
SVs joining an already bootstrapped network will take the initial round from their sponsor.

When initializing the network, this value is passed as an argument to the Daml choice `DsoRules_Bootstrap`.

## Motivations

The main motivation for this change is to reset TestNet with an initial round close to the one running in MainNet for the future resets.
MainNet continuously progresses through rounds and forcing TestNet to restart from round zero creates a significant gap, especially in the coin issuance.
We want it to align with MainNet.

### Risks and mitigations

There is no risk on MainNet as this code is only used on network initialization.

If SV1 uses the wrong initial round configuration, the network will be bootstrapped with the wrong round. 
This can be recovered by resetting again and fixing the initial round. This is not any worse than the current state where we always start with round zero, the wrong round.

## Backwards compatibility

If no `SPLICE_APP_SV_INITIAL_ROUND` is configured, the round is automatically set to 0.

## Reference implementation

An early version of this implementation can be found in [splice - feature/bootstrap-with-non-zero-round](https://github.com/hyperledger-labs/splice/tree/feature/bootstrap-with-non-zero-round)

## Copyright

This CIP is licensed under CC0-1.0: [Creative Commons CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/)

## Changelog

* **2025-07-09:** Initial draft of the proposal.
