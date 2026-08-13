---
inazip: 1
title: INAZIP purpose and process
author: Inazuma maintainers
status: Live
type: Informational
created: 2026-08-13
requires: none
---

## Summary

Defines what an INAZIP is, when one is required, and the path a proposal takes from
an idea to a rule the network actually follows.

## Motivation

Nodes stay on one chain only while they apply identical rules. If rules can change
because someone merged a pull request, then whoever controls the repository controls
the network — which defeats the purpose of running your own node.

Two things prevent that:

1. Rule changes are written down in public before they are implemented.
2. Rule changes only take effect at a block height, so validators adopt them by
   choosing to run a release, not by having it pushed on them.

INAZIPs are mechanism (1). Activation heights are mechanism (2).

## Specification

An INAZIP is a Markdown file in `proposals/`, with the front matter fields from
`template.md`, and these statuses:

| Status | Definition |
| --- | --- |
| Draft | Submitted, not yet reviewed in depth |
| Review | Under active discussion; specification being finalised |
| Accepted | Specification agreed; implementation may begin |
| Scheduled | Implemented and released, with an activation block published |
| Live | Activation block has passed on the public network |
| Rejected | Declined; reason recorded in the file |
| Withdrawn | Abandoned by the author |

An INAZIP is **required** for any change that alters:

- consensus rules, block format, or proposer selection
- transaction encoding, signature schemes, or address format
- state layout or state-root computation
- fees, rewards, staking, unbonding, slashing, or jailing parameters
- precompiles, host ABI semantics, or gas metering rules

An INAZIP is **not required** for bug fixes that restore documented behaviour, new
RPC methods, SDK or wallet features, documentation, tests, or tooling.

Types: `Core` (consensus and state), `Networking` (P2P and transport), `Interface`
(RPC, encodings, wallet-facing formats), `Economic` (supply, fees, rewards, slashing),
`Informational` (process and guidance, like this one).

Every Core, Networking, or Economic proposal that is not backwards compatible must
name an activation height in its Specification, and no proposal may apply new rules
to blocks that already exist.

## Rationale

The alternative — informal decisions in chat — produces exactly the failure this
process is designed to avoid: rules nobody can point to, changes nobody can audit,
and upgrades operators discover after they break.

Keeping rejected proposals in the repository is deliberate. The written reason is
what stops the same idea consuming review time repeatedly.

## Backwards compatibility

None required. This proposal describes process, not chain behaviour.

## Security considerations

The process itself is a security control. The mandatory security section, the
activation-height rule, and the second-reviewer requirement for changes touching
signatures, voting, or fund movement exist so that consensus-critical code cannot
land unexamined.

Emergency exploit fixes may bypass review and ship immediately, followed by a public
advisory within 72 hours and a retroactive INAZIP.

## Test plan

Not applicable.

## Reference implementation

This document.

## Copyright

Released into the public domain.
