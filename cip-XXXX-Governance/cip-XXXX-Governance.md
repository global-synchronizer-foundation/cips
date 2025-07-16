Please make a copy of this folder for any new CIP. Any artifacts for that CIP should go inside that folder. 


This template applies only to CIPs that assign Super Validator weights in return for specified deliverables. Other CIPs should use the standard template. 


More information can be found in [CIP-0000](../../cips/cip-0000/cip-0000.md).

## Title

<pre>
  CIP: <CIP number, or "?" before being assigned>
* Layer: <TBD> [TBD see Layer definition cip-XXXX](../../cips/blob/main/cip-XXXX).
  Title: <CIP title; maximum 44 characters>
  Author: <list of authors' real names>
* Discussions-To: <email address>
* Comments-Summary: <summary tone>
* Comments-URI: <links to wiki page for comments>
  Status: <Draft | Active | Proposed | Deferred | Rejected |
           Withdrawn | Final | Replaced | Obsolete>
  Type: Governance 
  Created: <date created on, in ISO 8601 (yyyy-mm-dd) format>
  License: <abbreviation for approved license(s)>
* License-Code: <abbreviation for code under different approved license(s)>
* Post-History: <dates of postings to [https://lists.sync.global/g/cip-discuss] mailing list, or link to thread in mailing list archive>
* Requires: <CIP number(s)>
* Replaces: <CIP number>
* Superseded-By: <CIP number>
</pre>

## Abstract

Add [name] as SV of Weight [#]

## About Applicant

Who is this party and why do we care?

## Deliverables for full SV Reward:
| Deliverable | Acceptance Criteria | Deadline | Weight Earned |
|-------------|---------------------|----------|---------------|
|     X       |         X           |   X days |     X.X       |
|     X       |         X           |   X days |     X.X       |



## SV Reward Mechanics: 
* An `extraBeneficiary` PartyID associated with the ‘escrowed’ Super Validator will be setup by the GSF with an SV Weight at the maximum earnable weight in the CIP that granted rights to that Super Validator.
    * The Applicant is responsible for all costs associated with the operation of the escrow SV
    * The escrow SV will NOT mint rewards on a block by block basis
    * All escrow SV rewards will go to the Unclaimed Rewards pool
* ⅔ of the Super Validator Operators will update their configurations to allow GSF to host the full weight to be earned by the given Super Validator
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

* This CIP is licensed under CC0-1.0: [Creative Commons CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/).

## Changelog

* **DATE:** Initial draft of the proposal.
