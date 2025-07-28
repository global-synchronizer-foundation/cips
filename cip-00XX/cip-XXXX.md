## Weighted Validator Liveness Rewards for SV-Determined Parties

<pre>
  CIP: ?
* Layer: Daml
  Title: Weighted Validator Liveness Rewards for SV-Chosen Parties
  Author: Moritz Kiefer
* Discussions-To: cip-discuss@list.sync.global
* Comments-Summary: N/A
* Comments-URI: N/A
  Status: Draft
  Type: Tokenomics
  Created: 2025-07-23
  License: CC0-1.0
* Post-History: N/A
</pre>

## Abstract

This CIP proposes extending the concept of validator liveness rewards
to arbitrary parties--not just validator operators--as well as
supporting weights defined through governance.

This allows hosting multiple parties on a single validator while still providing the same
rewards to the parties on the same node as if they were running their
own node. It also allows weighting certain some parties higher, e.g.,
because they have been shown to contribute significantly to the
success of the network, or have committed to do so. 

## Copyright

This CIP is licensed under CC0-1.0: [Creative Commons CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/)

Note: other licenses are avilable see recommendations in [cip-0000](../../cips/cip-0000/cip-0000.md)

## Specification

The existing functionality where each SV can grant a
`ValidatorLicense` to any validator operator by providing a validator
onboarding secret, is extended such that each SV can
grant a `ValidatorLicense` with the default weight to arbitrary
parties (including parties relying on external signing) through the SV
UI. 

In addition to that, SVs get the ability to change the weight of a
validator license to a non-negative decimal. The liveness rewards
minted by a license with weight N are equivalent to the liveness
rewards minted by N licenses.

### SV UI

The SV UI is extended with three different functionalities:

1. An SV can grant a validator license to a user-specified party id.
2. SVs can vote on weight changes for an existing validator license.
3. SVs can vote on withdrawing a validator license.

### Daml

- At the Daml level there is no distinction between validator operator
  parties and any other parties. So the existing choices that allow an
  SV to grant a validator license to validator operator parties can
  already be used to grant licenses to arbitrary parties.
- To support automatic reward minting for parties relying on external
  signing, a new `MintingDelegation` contract is added which allows
  signing an transaction once which delegates the right to create and
  mint liveness activity records to a validator operator party. This
  then allows the automation in the validator app to submit the
  required transactions each round on behalf of the external
  party. Note that `MintingDelegation` is limited to creating and
  minting activity records, it does not grant the delegate rights to
  do anything else on behalf of the party. Minting delegations can be
  created and accepted through the splice wallet UI for local parties
  or through the ledger API endpoints for [external transaction
  signing](https://docs.digitalasset.com/build/3.3/tutorials/app-dev/external_signing_submission.html)
  for parties relying on external signing.
- To support weighting of liveness rewards, the `ValidatorLicense` and
  `ValidatorLivenessActivityRecord` are extended with an optional
  weight field. If not set, as is the case for all existing licenses,
  it defaults to weight 1.

## Motivation

MainNet is close to the maximum number of validators that can be
supported. This is driven to a significant degree due to spinning up a
separate validator being the only option to get more liveness rewards.
By supporting liveness rewards for non-validator operator parties,
this pressure is reduced without impacting the rewards that users can
receive.

Weighting allows balancing the benefit to the network coming from
users that do nothing other than accumulating validator rewards with the benefits provided by parties
that significantly contribute to the success of the network.

## Rationale

Manual weighting through votes provides a simple mechanism to balance
the contributions of different parties to the network. While more
complex automatic mechanisms based on staking, measuring activity in
apps or similar could be built, those all have vastly more
implementation complexity and could be added on top of this CIP by
automating votes on weight changes.

Allowing SVs to unilaterally grant a license after approval from the
tokenomics committee with default weight to any party is already
supported by the current Daml code and consistent with being able to
unilaterally grant it to validator operators. If this gets abused,
other SV operators can withdraw validator licenses or set their weight
to 0.

Weight changes on the other hand require more care and so those require a vote.

## Backwards compatibility

The changes are all backwards compatible. However, like any Daml change
it introduces a soft fork where all nodes on the network must upgrade
to the version that introduced this change before it becomes effective.

## Reference implementation

The reference implementation of the Daml changes is available in two draft PRs.

1. [weighting of validator licenses](https://github.com/hyperledger-labs/splice/pull/1634)
2. [delegated reward minting](https://github.com/hyperledger-labs/splice/pull/1627)
