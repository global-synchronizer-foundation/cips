## CIP 0067

<pre>
  CIP: CIP 0067
  Title:  One-Time Allocation of Historical Unclaimed Rewards to GSF
  Author: Eric W Saraniecki 
  Status: Draft 
  Type: Meta / Tokenomics 
  Created: 2025-06-13
  Approved: TBD
  License: CC0-1.0
</pre>

## Summary:
This proposal authorizes a one-time allocation of **historical unclaimed rewards** from the Canton validator reward pool to the **Global Synchronizer Foundation (GSF)**. These funds compensate GSF for its critical contributions to the design, governance, and operational stability of the Global Synchronizer.

## Motivation
GSF has played a foundational role in establishing and maintaining the systems that govern the Canton Network, including:

* Launching the legal entity, onboarding founding members, and establishing governance processes for the Global Synchronizer
* Defining and maintaining long-term technical roadmaps for protocol coordination
* Structuring pathways for meaningful community engagement and contribution
* Leading coordination and execution of Super Validator governance and network-wide decision-making

## Scope and Exclusions:
* **Included**: 

    * All **unclaimed** validator rewards **as of the effective date** of this proposal that are **not** explicitly reserved under any approved **milestone-based SV reward schedule**.
    * At time of drafting this note, it is expected that 1.6 to 1.7 billion CC will transfer to the GSF from this action 

* **Excluded**: 

    * Any **future** unclaimed rewards 
    * Rewards currently held in escrow or contractually earmarked for **SV milestone-based delivery**


## Specification: 
* **Recipient**: Global Synchronizer Foundation Treasury (designated on-chain address)
  * The Global Synchronizer Foundation to supply the appropriate on-chain address for delivery, the GSF Tokenomics Committee to verify the address prior to the transfer.
* **Transfer Mechanism**: One-time disbursement authorized via governance
  * Prior to initiating any send of assets, the GSF Tokenomics committee must manually review the designated address and confirm it is accurate.
  * The GSF Tokenomics Committee to leverage best practices for the relevant disbursement which shall include:
    * At least one successfully completed de minimis sized transfer from the unclaimed pool to the designated Global Synchronizer Foundation address.  Any such test transfer must be confirmed to have been properly received by the designated address. Any failures for this test send must be resolved and subsequently successfully retested before sending the full balance.
    * At least one successfully completed de minimis sized transfer from the unclaimed pool to the designated Global Synchronizer Foundation address.  Any such test transfer must be confirmed to have been properly received by the designated address. Any failures for this test send must be resolved and subsequently successfully retested before sending the full balance.
  * Upon successfully completely a test transfer as described above, the remaining balance may be sent to the designated address.
    * The Global Synchronizer Foundation and GSF Tokenomics committee independently confirm the expected CC have been received.
* **Reference Date**: Snapshot to be taken at a designated block height or timestamp to establish the reward pool eligible for reallocation

## Actions: 
* GSF SV Operator to calculate amount of unclaimed rewards to be assigned to the GSF as of a specific block height
* GSF SV Operator to initiate on-chain process for GSF to mint unclaimed coins when ready

## Copyright

This CIP is licensed under CC0-1.0: [Creative Commons CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/).

## Changelog

* **2025-06-13:** Initial draft of the proposal.
* **2025-07-10:** Update proposal based on cip-discuss feedback.

