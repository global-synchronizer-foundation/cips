# test for denex.io

<pre>
  CIP: test for denex.io
  Title: test for denex.io
  Author: Eric Saraniecki
  Status: Approved
  Type: Governance
  Created: 2025-11-07
  Approved: 2025-11-24
  License: CC0-1.0: Creative Commons CC0 1.0 Universal
</pre>

## Abstract
test for denex.io
## About 
test for denex.io

## SV Reward Mechanics: 
* An `extraBeneficiary` PartyID associated with the ‘escrowed’ Super Validator will be setup by the Foundation, or another SV node operator approved to provide SV rewards escrow services, with an SV Weight at the maximum earnable weight. 
    * The Applicant is responsible for coordinating the process of setting up the escrowed weights with the GSF and the operator of the SV node.
    * The Applicant is responsible for all costs associated with the operation of the escrow SV
    * The escrow SV will NOT mint rewards on a block by block basis
    * All escrow SV rewards will go to the Unclaimed Rewards pool
* ⅔ of the Super Validator Operators will update their configurations to allow the escrowing SV node to host the full weight to be earned by the given Super Validator
* Applicant is required to present proof of successful completed milestones to the Tokenomics Working Group
    * Applicant is required to present a calculation for number of Canton Coin it should earn for meeting the requirements of the milestone
* If the Tokenomics Working Group agrees the milestone has been met and agrees with the calculation, an announcement will be sent via the Tokenomics-Announce mailing List
    * The GSF will update the `extraBeneficiary` to an active PartyID controlled by that Super Validator. 
    * ⅔ of Super Validator Operators will then assign a portion of the Unclaimed Rewards to be minted by the Applicant’s Validator, based on the calculation approved by the Tokenomics working group.
   
* If any milestones and associated rewards are not achieved by the deadline
    * Applicant will be notified they have not met a deliverable by the GSF 
    * Remaining SV Weight assigned to the `extraBeneficiary` SV will be removed from the GSF node configuration, and the total SV weight of the GSF SV node will be reduced by the same amount by a vote of the Super Validators.
    * The Tokenomics Working Group will make a recommendation to the SVs on what to do with the Unclaimed Rewards 
* Applicant is subject to CIP-0045 : SV Operating Requirements
    * If, at any time, the Applicant has been rewarded SV Weight > 2.5, they are required to operate their SV within 6 months of crossing that Weight. This SV node will join the network with an SV weight of zero (0) and may add weights as the SV completes the milestones listed in this CIP.

## Copyright
This CIP is licensed under **CC0-1.0: Creative Commons CC0 1.0 Universal**.

## Changelog 

2025-11-07: Initial draft of the proposal.
2025-11-24: CIP approved.
