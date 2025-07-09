## CIP XXXX

<pre>
 CIP: CIP XXXX
 Title: Bootstrap splice node from non-zero round
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
This approach aims to better align with MainNet's ongoing state.

## Specification

### Daml

A new optional argument `initialRound` is added to the `DsoRules_Bootstrap` choice. 
When provided, this value is used as the initial round in the network and changes the issuance periods accordingly.

### SV Application

The SV Application is modified so that it reads the new optional flag `SPLICE_APP_SV_INITIAL_ROUND` defaulted to 0.
Each SV Application compares its value to its onboarding sponsor's value before to save it to the user metadata to ensure
all SV nodes use the same initial round.

The founding node passes its value as argument of the Daml choice `DsoRules_Bootstrap`. 

### Scan Application

Scans reads the initial round from its SV user metadata.

The `ScanAggregator` component within the Scan Application is updated to consider the initial round as the first round.
Instead of backfilling and aggregating data from round 0, the initial round will determine the starting point.

## Motivations

The main motivation for this change is to reset TestNet with an initial round close to the one running in MainNet for the upcoming TestNet reset scheduled in September.
MainNet continuously progresses through rounds and forcing TestNet to restart from round zero creates a significant gap, especially in the coin issuance.
We want it to align with MainNet.

### Risks and mitigations

On every SV Application initialization, it checks whether the configured initialRound matches its sponsor SV's initial round
to ensure all SV Applications use the same round. This round is dumped into its user metadata so that scan can read it.
SV applications that do not set the initial round correctly will not be able to start.

## Backwards compatibility

If no `SPLICE_APP_SV_INITIAL_ROUND` is configured, the round is automatically set to 0.

## Reference implementation

An early version of this implementation can be found in [splice - feature/bootstrap-with-non-zero-round](https://github.com/hyperledger-labs/splice/tree/feature/bootstrap-with-non-zero-round)

## Copyright

This CIP is licensed under CC0-1.0: [Creative Commons CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/)

## Changelog

* **2025-07-09:** Initial draft of the proposal.
