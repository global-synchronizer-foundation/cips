---
Author: PixelPlex Inc.
CIP: TBD
Created: 2026-01-27
License: CC0-1.0
Status: Draft
Title: Canton Network Party Profile Credentials
Type: Standards Track
---

# Abstract

This CIP standardizes a portable representation of **party profile
metadata** for user‑interface rendering on the Canton Network based on
the [**Canton Network Credentials Standard**](https://github.com/canton-foundation/cips/pull/204).

It defines:

-   A set of **standard claim keys** for common party metadata such as
    display name, avatar, website, and optional contact/social
    information.
-   A clear **application-side interpretation model** for rendering these
    claims consistently in user interfaces.

The profile claims defined by this CIP are **informational only** and
**MUST NOT** be interpreted as verified identity attributes or used for
name or recipient resolution.

# Motivation

Party IDs serve as the primary identity anchors on the Canton Network.
While suitable for infrastructure-level identification, they are
difficult for humans to distinguish and remember.

Applications interacting with Canton often need to display meaningful
information about the party behind a party ID, such as:

- a human-readable display name
- an avatar
- a website
- contact information
- social handles

Currently there is no standardized way to represent or resolve such
metadata across applications.

This CIP introduces a **standardized profile metadata model** that
allows:

-   party owners to publish profile information
-   applications to interpret profile claims consistently
-   users to distinguish parties in wallets, explorers, and applications

This improves **user experience and interoperability across Canton
ecosystem applications**.

# Specification

This CIP builds on the **Canton Network Credentials Standard** and uses
its claim encoding and Credential Lookup API semantics.

Profile credentials defined by this CIP MUST be self-published: the
credential `issuer` and `holder` MUST be equal. Applications MUST ignore
profile claims from credentials where `issuer != holder`.

Every claim defined by this CIP describes the credential holder. Claim
keys defined by this CIP MUST NOT contain an explicit `!subject` suffix,
and applications MUST ignore such a claim if the suffix is present.

Self-publication demonstrates that the holder authorized publication of
the claims. It does not verify the claims' accuracy, establish legal
identity, or prove ownership of an external account or resource.

Profile metadata is expressed as **credential claims** under a dedicated
namespace:

`cip-<nr>/`

Where `<nr>` will be replaced by the assigned CIP number.

# Party Profile Claim Keys

For social fields, this CIP uses grouped claim keys (for example,
`cip-<nr>/social:discord`) to align naming conventions with
[CPRP (CIP PR #171)](https://github.com/canton-foundation/cips/pull/171)-style
field organization while keeping this CIP namespace prefix.

## `cip-<nr>/display-name`

Human-readable name used for UI display.

Credential producers:

-   MUST encode the value as a Unicode string of no more than **64 Unicode characters**

Applications:

-   MUST treat the value as a display string
-   MUST NOT treat it as an identifier
-   SHOULD render the value as-is after applying UI escaping
-   MAY gracefully truncate the rendered value

The kebab-case `display-name` property follows the naming convention used
by the Canton Network Credentials Standard.

## `cip-<nr>/avatar`

Avatar reference for UI rendering.

Credential producers:

-   MUST encode a URI conforming to RFC 3986
-   MUST use an `https://` URL or `ipfs://` URI

Applications:

-   MUST treat the value as a reference to an avatar resource
-   MUST NOT interpret the value as identity verification
-   MUST validate the URI before using it
-   MAY ignore values that are not valid or supported URIs

## `cip-<nr>/website`

Website reference associated with the party.

Credential producers:

-   MUST encode an absolute `https://` URL conforming to RFC 3986

Applications:

-   MUST treat the value as informational metadata
-   MUST NOT treat it as an authoritative identifier
-   MUST validate the URL before presenting it as a link
-   MAY display invalid URLs as plain text
-   MUST NOT treat invalid values as trusted links

## `cip-<nr>/email`

Informational contact email.

Credential producers:

-   MUST encode a value conforming to RFC 5322 `addr-spec` syntax

Applications:

-   MUST treat this value as informational metadata
-   MUST NOT assume ownership or verification
-   MUST validate the value before rendering a `mailto:` link
-   MAY render a valid value as a `mailto:` link

## `cip-<nr>/social:telegram`

Telegram handle.

Credential producers:

-   MUST encode a value conforming to Telegram username format rules
-   MUST encode the value **without an `@` prefix**

Applications:

-   MUST treat the value as a Telegram username
-   MUST validate the value before constructing a link
-   MAY render the handle with a leading `@`

## `cip-<nr>/social:x`

X (Twitter) handle.

Credential producers:

-   MUST encode a value conforming to X username format rules
-   MUST encode the value without a leading `@`

Applications:

-   MUST treat the value as an X username
-   MUST validate the value before constructing a link
-   MAY render it with `@`

## `cip-<nr>/social:github`

GitHub username or organization.

Credential producers:

-   MUST encode a value conforming to GitHub username or organization name format rules

Applications:

-   MUST treat the value as informational metadata
-   MUST validate the value before constructing a link
-   MAY link to a GitHub profile URL

## `cip-<nr>/social:discord`

Discord handle or user ID.

Credential producers:

-   MUST encode a value conforming to Discord username or user ID format rules

Applications:

-   MUST treat the value as informational metadata
-   MAY display the value as provided

# Party Profile Resolution

Applications MUST retrieve party-profile credentials through the
Credential Lookup API defined by the Canton Network Credentials Standard.
A lookup MUST constrain `holder` to the party whose profile is requested
and MUST constrain or validate `issuer` so that it equals the holder.
Applications SHOULD constrain `keyPrefix` to `cip-<nr>/` when only profile
properties are required.

Applications MUST use only active credential records. A credential MUST
be ignored when the lookup time is before `validFrom`, after `validUntil`,
or after the registry record's `expiresAt`, when the respective field is
present. Archived credentials MUST be ignored. The precise time-boundary
semantics MUST follow the Canton Network Credentials Standard.

Applications SHOULD query the DSO Credential Registry by default and MAY
query additional registries according to an explicit local policy. When
multiple registries are queried, applications MUST apply a deterministic
registry-priority order.

Profile properties MUST be resolved independently. After the filtering
above, a value from a higher-priority registry takes precedence. Within
one registry, the credential with the latest registry record time takes
precedence. If record times are equal, the contract ID MUST be used as the
deterministic tie-breaker in the order defined by the Canton Network
Credentials Standard.

The resolution/composition flow defined in
[CPRP (CIP PR #171)](https://github.com/canton-foundation/cips/pull/171)
MAY be used as a higher-level policy only if it preserves the mandatory
lookup, filtering, and deterministic resolution requirements above.

# Rationale

## Why Party-Centric Profiles

Profiles are attached directly to the **party identity anchor**,
ensuring stability across applications and identity flows.

A self-published profile establishes that its publication was authorized
by the credential holder, but its contents remain unverified informational
metadata. Applications SHOULD distinguish self-published profile metadata
from independently verified identity credentials.

## Why Namespaced Claim Keys

Using namespaced keys `cip-<nr>/*`:

-   minimizes protocol changes
-   preserves compatibility with existing credential infrastructure
-   enables efficient querying
-   allows extensibility

Future well-known profile attributes may be standardized via amendments
to this CIP (or a separate CIP where appropriate) while continuing to
use the `cip-<nr>/` namespace.

This namespace design is primarily for forward compatibility:
applications may ignore unrecognized keys without breaking.

## Why Application-Side Resolution

Different applications may have different trust policies, issuer
preferences, and registry priorities.

Therefore this CIP standardizes only:

-   claim namespace
-   claim interpretation semantics
-   credential filtering
-   deterministic per-property resolution

but leaves configurable:

-   selection of additional registries
-   registry priority ordering

# Examples

## Example 1 --- Basic Profile Claims

A self-published credential, for which `issuer = holder`, contains:

-   `cip-<nr>/display-name = "PixelPlex"`
-   `cip-<nr>/avatar = "https://cdn.example.com/profiles/pixelplex.png"`
-   `cip-<nr>/website = "https://pixelplex.io"`
-   `cip-<nr>/email = "info@pixelplex.io"`
-   `cip-<nr>/social:github = "pixelplex"`

Applications should render these values as profile metadata only and
must not treat them as authoritative identity identifiers.

## Example 2 --- Social Handle Rendering

A credential contains:

-   `cip-<nr>/social:telegram = "Pixelplex"`
-   `cip-<nr>/social:x = "pixelplexinc"`

Applications may render these in UI as `@Pixelplex` and
`@pixelplexinc`
while producers encode the claim values without the leading `@`.

## Example 3 --- Invalid Website Value

A credential contains:

-   `cip-<nr>/website = "not-a-url"`

This value does not conform to the producer requirements. Applications
may display it as plain text but must not treat it as a trusted link.

# Backwards Compatibility

This CIP is **fully additive** relative to the Canton Network Credentials
Standard.

-   No changes to Canton protocol
-   No changes to Daml models beyond those introduced by the Canton Network Credentials Standard
-   No changes to registry contracts beyond those introduced by the Canton Network Credentials Standard

Applications not implementing this CIP will treat the claims as generic
key--value metadata.

# Reference Implementation

Not required.

This CIP specifies only claim keys and application-side interpretation
rules.
