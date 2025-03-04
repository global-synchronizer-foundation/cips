## CIP XXXX

<pre>
  CIP: ?
  Title: Changes to on-ledger vote logic
  Author: Itai Segall
  Comments-Summary: No comments yet
  Status: Draft
  Type: Governance
  Created: 2025-03-04
  License: CC0-1.0
</pre>

## Abstract

As the Super Validators developed experience with on-ledger voting, we realized that the current
implementation is too complex, error-prone, and hard to reason about. This CIP proposes a new
process, and underlying Daml code changes, that will make governance processes on-ledger much
more user-friendly and intuitive. In short:
a) votes on config changes will change individual (or a few) config values, and not the whole set of configurations;
b) vote proposals will have an expiration date, after which if not enough votes have been submitted,
they will be auto-rejected;
c) votes will remain open for editing until they become effective;
d) votes will be early-rejected upon 2/3 majority of reject votes;
e) vote requests can be labeled as "immediate effectivity",
meaning they will automatically get accepted and take effect immediately as soon as 2/3 of the
Super Validator nodes vote in favor.

## Copyright

This CIP is licensed under CC0-1.0: [Creative Commons CC0 1.0 Universal](https://creativecommons.org/publicdomain/zero/1.0/)

## Specification

### Vote on Individual Configuration Values

In the current implementation, every vote on config changes (e.g. the AmuletConfig, which governs
mining, fees, etc) replaces the entire set of configuration values from the current set to those
specified in the vote request. This has often caused confusion when multiple vote requests were
created roughly at the same time, but their interaction with each other was hard to reason about,
ending up with conflicts, and with votes that accidentally overwrote each other's values. In the
new implementation, a vote request will only apply changes to those fields that the proposer has
chosen to change, leaving the others unmodified, regardless of their value at the time of applying
the change.

### Expiration as a Due Date for Enough Votes

Every vote request will have an expiration date. This date should be thought of as the due date for
Super Validator operators to act on this vote request. If at the time of expiration, not enough
votes are in (i.e. less than 2/3 of the Super Validators have put in any vote), then the vote request
is immediately considered stale and auto-rejected.

### Votes Remain Open

In the current implementation, once the expiration date on a vote request has passed,
votes on it can no longer be modified. In practice, this has proven to be a burdening
limitation, and Super Validator operators often expressed a need to modify their vote
until the very last minute, i.e. until the corresponding action as become effective.
The new implementation will satisfy that requirement by allowing any vote to be modified
until the point where the vote has become effective.

### Early Rejection

Mistakes happen. We have had several cases on ledger already where vote requests contained
errors, and we quickly realized they should get rejected. In the current implementation,
there is no way to quickly get rid of those votes despite wide agreement that they should
be rejected. In the new implementation, vote requests that collected 2/3 "Reject" votes will
immediately close as rejected.

### Immediate Effectivity

Currently, any vote request has both an expiration date and effectivity date. We propose to introduce
an "immediate effectivity" option, which does not specify an explicit effectivity date, but instead
becomes effective immediately upon collecting 2/3 "Accept" votes, but not earlier than the
expiration date. This would allow taking relatively quick actions, without having to wait after the
expiration date, and without having to put much thought into chosing an effectivity date when
creating the vote request.


## Motivation

The changes propsed here are all improvements to the operational governance process, based on
the collective experience that the Super Validator operators have developed over the past months.

## Backwards compatibility

Vote requests are short-lived. The changes proposed here will apply only to vote requests created
after these changes are applied, and leave past votes intact. The UI of the SV application will
be adapted to match the new logic, but will come into use in sync with the changes in the Daml
models described here.

## Reference implementation

We have started implementing the required changes for this CIP, which include changes in the
governance Daml models, as well as the backend and frontend of the SV application . Access to a test
environment will be provided to all Super Validator Operators to experiment with and comment on
before we finalize the code and finalize the changes.
