---
inazip: <leave blank; an editor assigns this>
title: <short, descriptive, no marketing>
author: <name or handle>
status: Draft
type: <Core | Networking | Interface | Economic | Informational>
created: <YYYY-MM-DD>
requires: <INAZIP numbers, or none>
---

## Summary

Two or three sentences. What changes, in plain language, understandable by someone
who runs a validator but does not read Rust.

## Motivation

What problem exists today? Who is hurt by it — users, validators, developers? Show
the concrete situation. If there is data (block times, fee levels, failure rates),
put it here.

## Specification

The exact rules. This section must be precise enough that two people could
implement it independently and produce identical blocks.

Include, where relevant:

- New or changed data structures, with field names, types, and byte sizes.
- Encoding and hashing rules, including endianness and domain separation.
- Validity rules: what makes a transaction or block invalid.
- Parameters and their exact values.
- Behaviour before and after the activation height.

```text
if height >= ACTIVATION_HEIGHT {
    <new rule>
} else {
    <old rule, unchanged>
}
```

## Rationale

Why this design and not the obvious alternatives? Name the alternatives you rejected
and why. Trade-offs you accepted go here.

## Backwards compatibility

- Do old nodes reject new blocks? If yes, this is a consensus break and needs an
  activation height and a release plan.
- What breaks for wallets, explorers, indexers, and the SDK?
- What must node operators do, if anything?

## Security considerations

New attack surface, and what stops it. Cover at least:

- Can this be used to spend funds that are not yours, or to create supply?
- Can it be used to stall or split consensus?
- Can it be used to exhaust a node's CPU, memory, disk, or bandwidth cheaply?
- Does it change what an attacker learns about users?

If the honest answer to something is "unknown", write that.

## Test plan

How correctness is demonstrated: unit tests, a devnet run, replaying historical
blocks, load numbers, fuzzing.

## Reference implementation

Link the pull request or branch, or write "none yet".

## Copyright

Released into the public domain.
