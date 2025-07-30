## CIP 00XX

<pre>
  CIP: CIP 00XX
  Title:  Enabling Delegated Participation in Liveness Rewards
  Author: Eric W Saraniecki 
  Status: Draft 
  Type: Meta / Tokenomics 
  Created: 2025-06-13
  Approved: TBD
  License: CC0-1.0
</pre>

## Summary:
This proposal outlines a set of capabilities the Canton Network should support to allow any Party—regardless of whether they operate a validator node—to directly participate in Liveness rewards. These capabilities are designed to increase participation, improve flexibility in reward allocation, and provide governance mechanisms to better incentivize desired behaviors across the network.

## Motivation
Today, access to Liveness rewards is strictly tied to the operation of validator infrastructure. This design limits the number of Parties who can be rewarded for contributing to network growth and creates operational friction for those relying on third-party node operators.

The Canton Network is also expected to face persistent supply constraints on validator infrastructure. Demand to participate—driven by reward mechanisms—will likely exceed the number of validator slots available at any given time. Decoupling participation in rewards from node operations is a necessary step toward scaling validator engagement and increasing the flexibility of governance.

## Proposed Capabilities:
We propose that the network enable a new ability to participate in Liveness rewards, with the following key properties:

* **Direct Reward Participation** Any explicitly designated Party should be able to receive Liveness rewards, even if they are not operating a validator node themselves. This opens the door to broader participation and allows for delegation or outsourcing of infrastructure. 
* **Reward Revocability** The right to earn Liveness rewards should be revocable. This creates an enforcement mechanism that governance bodies or network entities (e.g. Super Validators) can use to ensure participants meet agreed-upon standards of behavior or contribution. 
* **Weighted Participation** The rewards allocated to a Party should be able to vary in weight or proportion. This enables the network to reward participants based on merit, stake, reputation, or any other governance-defined metric. 
* **Incentive Governance** These capabilities should serve as levers for the network to govern participation—encouraging valuable behavior, penalizing non-participation, and gradually evolving toward more dynamic and programmable reward systems.

## Strategic Benefits: 
* **Scalability of Participation**: Reward pathways are no longer bottlenecked by infrastructure capacity.
* **Improved Incentive Alignment**: Participants can be rewarded for value creation even if they don't run infrastructure. 
* **Foundation for Staking Models**: Weighted participation allows future integration with CC-based staking or slashing models. 
* **Governance Leverage**: Revocability and weighting introduce new governance tools for shaping behavior and enforcing standards. 

## Next Steps: 
1. **Community Alignment**: Socialize this proposal with Tokenomics Working Group 
2. **Design Specification**: Develop a specification for how these capabilities could be implemented without prescribing mechanisms prematurely
3. **Formal CIP**: Finalize and submit for approval

## Copyright

This CIP is licensed under CC0-1.0: [Creative Commons CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/).

## Changelog

* **2025-06-13:** Initial draft of the proposal.
