
  ## CIP:  CIP-00XX
<pre>
  Title: Synchronizer Migration with Downtime to Canton 3.3
  Author:
    Curtis Hrischuk
    Marcin Ziolek
    Soren Bleikertz
    Wayne Collier
  License: CC-1.0
  Status: Draft
  Type: Standards
  Created: 2025-05-14
</pre>

## Abstract

Splice 0.4.0 introduces new features that rely on upgrading from Canton 3.2 to Canton 3.3. This upgrade require a Canton protocol version upgrade, which will require a Major Upgrade with Downtime action by the Super Validators and Validators.

## Copyright

This CIP is licensed under CC0-1.0: [Creative Commons CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/)

## Specification

Super Validators will upgrade from one Canton release to the other, e.g., 3.2 to 3.3 requiring a special upgrade procedure described under [Synchronizer Upgrades with Downtime](https://docs.dev.sync.global/sv_operator/sv_major_upgrade.html#sv-upgrades). This process requires a synchronized operational procedure involving all Super Validator nodes, and pauses the synchronizer until the upgrade is complete. While paused, no transactions will be sequenced on the synchronizer, and thereby also no activity records are created or rewards minted. This process has been tested extensively and has been completed twice previously on DevNet, TestNet and MainNet, for Canton 3.1 and Canton 3.2.  The procedure will be tested for Canton 3.3 on DevNet and TestNet before it will be carried out on MainNet. The procedure typically completes within three hours.

Validators undergo a similar but simpler procedure to upgrade their nodes described in the [validator documentation](https://docs.dev.sync.global/validator_operator/validator_major_upgrades.html). Validators do not have to do this synchronized with SVs, but until they upgrade, their node will be unable to create new activity records or mint rewards.

The proposed dates for the upgrade are:
- MainNet: *Jun 25, 2025 1:00 PM GMT*
- TestNet: *Jun 11, 2025 1:00 PM GMT*
- DevNet: *May 28, 2025 1:00 PM GMT*

The specific actions to be taken on chain by the Super Validator operators are:

Using the “*Set DsoRules Configuration*” vote proposal form, propose a vote that:

**DevNet**
- Check the checkbox for “*Set next scheduled domain upgrade*”
- Set “*nextScheduledSynchronizerUpgrade.time*” to 2025-05-28T13:00:00Z
- Set “*nextScheduledSynchronizerUpgrade.migrationId*” to 2.

Then, starting at *2025-05-28T13:00:00Z*, complete the documented procedure.  

**TestNet**
- Check the checkbox for “*Set next scheduled domain upgrade*”
- Set “*nextScheduledSynchronizerUpgrade.time*” to 2025-06-11T13:00:00Z
- Set “*nextScheduledSynchronizerUpgrade.migrationId*” to 4.

Then, starting at *2025-06-11T13:00:00Z*, complete the documented procedure.  

**MainNet**
- Check the checkbox for “*Set next scheduled domain upgrade*”
- Set “*nextScheduledSynchronizerUpgrade.time*” to 2025-06-25T13:00:00Z
- Set “*nextScheduledSynchronizerUpgrade.migrationId*” to 3.

Then, starting at *2025-06-25T13:00:00Z*, complete the documented procedure.  

## Motivation

The 0.3.x  Splice codebase has evolved since the Global Synchronizer nodes (the synchronizer nodes as well as the Super Validator and validator participant nodes) adopted version Canton 3.2.  The Canton 3.3 release improves the feature set of Canton 3.2 in multiple aspects. Notable improvements from an ecosystem growth perspective are: :
- The Token Standard is based on Daml interfaces. Implementing these interfaces in existing Daml templates like Amulet is not possible in Canton 3.2, as it disallows adding interface implementations to existing templates as part of a Smart Contract Upgrade. Canton 3.3 removes this limitation.
- Many of the choices in the Token Standard are dependent on ledger time. Expressing these constraints using getTime limits the maximal delay between preparing a transaction and submitting it for execution to one minute. Canton 3.3 introduces new primitives for asserting constraints on ledger time, which remove that artificially tight one minute bound, and instead capture the actual dependency of the submission time and ledger time. These new primitives are used in the `Amulet` implementation. 
- Composing Canton Network applications is best done via interfaces to minimize the required coordination for rolling out smart contract upgrades of dependencies. In Canton 3.2, this did not work well, as the version of Daml packages were chosen using purely local information about the submitting validator node instead of considering the packages vetted by the counter-participants involved in the transaction. Canton 3.3 introduces support for vetting-state-aware package selection to lower the required coordination for rolling out smart contract upgrades in many cases to zero.
- Application developers need stable and easily accessible APIs to build on. Canton 3.3 takes a major step forward in stabilizing the developer facing APIs of the Canton participant node by making all calls available in the gRPC Ledger API also accessible via HTTP JSON with OpenAPI schema definitions being provided for easy discoverability. Thus allowing developers to freely pick between gRPC or HTTP JSON depending on what suits them best.
- The Canton 3.3 release also introduces experimental Daml support for verifying cryptographic signatures. Thereby making it easier to build Daml workflows that bridge between Canton Network and other chains.

## Rationale 
The Canton 3.3 release is available for upgrade.  It enables new features in Splice 0.4.0, as well as new capabilities to be leveraged by Canton Network application developers and partners. A preview of the Canton 3.3 release notes is available [here](https://blog.digitalasset.com/developers/release-notes/canton-daml-3.3-preview).

The Canton 3.3 release introduces the following new capabilities:
- Easier application upgrades:  [Smart contract upgrading (SCU)](https://docs.daml.com/upgrade/smart-contract-upgrades.html#smart-contract-upgrade) that allows Daml models (packages in DAR files) to be updated transparently, provided guidelines in making application changes are followed.  
- Support for the Canton Network Token Standard. SCU allows the CNTS to evolve without breaking changes.                                                                      
- Enhancements for the interactive submission service (e.g., Time boundaries functions).  The interactive submission service is enabled by default.
- The Canton JSON APv2 is available which is as fully featured as the gRPC LAPI: mirroring the LAPI’s gRPC functionality (e.g., ability to submit batches of commands, Daml interfaces). It follows industry standards such as [AsyncAPI](https://docs.digitalasset.com/build/3.3/explanations/json-api/asyncapi) (websocket) and [OpenAPI](https://docs.digitalasset.com/build/3.3/explanations/json-api/openapi.html) (synchronous, request-response), which enables the use of the associated tooling (e.g., generate language bindings at the discretion of the developer). 
- A new approach to export and import of ACS snapshots, which is an improvement for any future [Synchronizer Upgrades with Downtime](https://docs.dev.sync.global/sv_operator/sv_major_upgrade.html#sv-upgrades), and will prepare Canton for zero-downtime protocol upgrades.
- A better package selection algorithm to select the package version that is common to  all validators that participate in a transaction,  when  a Dapp has multiple package versions deployed.  The new logic leverages the topology state of connected synchronizers to optimally select packages for transactions, ensuring they pass vetting checks on counter-participants.
- Improvements to the offline root namespace key procedure.   Scripts are provided to initialize a participant node's identity using an offline root namespace key.
- The Daml language introduces crypto primities which are useful for building bridges or wrapping tokens.
- KMS drivers are now supported in the Canton community edition to allow custom integrations with KMS

There are also numerous smaller robustness and feature improvements that will be described in the 3.3 release notes.

## Backwards compatibility

Applications that use the Validator APIs and Scan APIs are not affected (see [validator node network diagram](https://docs.dev.sync.global/validator_operator/validator_helm.html#validator-network-diagram)). Applications that integrate directly with the Canton participant node and its Ledger API should review the [Canton / Daml 3.3 Release Notes Preview](https://blog.digitalasset.com/developers/release-notes/canton-daml-3.3-preview) blog post for any updates they need to make.

## Reference implementation

The Splice 0.4.0 code can be found [here](https://github.com/hyperledger-labs/splice/tree/main).

The OSS Canton 3.3 code can be found [here](https://github.com/digital-asset/canton/tree/release-line-3.3).

Splice 0.4.0 and Canton 3.3 have been integrated and are completing functional and performance testing.   The token standard has been validated against Splice 0.4.0 and Canton 3.3 deployed on a separate, isolated DevNet.   Canton 3.3 Release Notes will be published once the release process is complete.

Changelog

* **2025-05-20:** Initial draft of the proposal.