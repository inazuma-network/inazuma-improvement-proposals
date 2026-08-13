---
inazip:
title: Staged verification and enforcement of ML-DSA-65 co-signatures
author: Inazuma maintainers
status: Draft
type: Core
created: 2026-08-13
requires: 1
---

## Summary

Transactions already carry an optional ML-DSA-65 (FIPS 204) co-signature next to the
Ed25519 signature. This proposal makes nodes verify that co-signature when present,
lets an account bind itself to a quantum key so classical-only spends are refused,
and finally makes the co-signature mandatory — each step at its own activation
height.

## Motivation

Elliptic-curve signatures fall to Shor's algorithm on a sufficiently large quantum
computer. Two properties make waiting dangerous:

1. Public keys are permanently public once an account transacts, so an attacker can
   harvest now and forge later.
2. Dormant accounts cannot be asked to migrate, so a post-hoc fix cannot protect
   them.

Wallets already produce the quantum half, and blocks already store it, so the
remaining gap is purely that nodes do not yet check or require it.

## Specification

Fields already present on a transaction: `pq_scheme` (`"ml-dsa-65"`), `pq_pubkey`
(1,952 bytes), `pq_signature` (3,309 bytes). The signed message is the existing
canonical transaction encoding, byte-identical to what Ed25519 signs.

### Phase A — verify when present (height `PQ_VERIFY_HEIGHT`)

For `height >= PQ_VERIFY_HEIGHT`:

- If all three `pq_*` fields are absent, apply current rules unchanged.
- If any is present, all three must be present, `pq_scheme` must equal
  `"ml-dsa-65"`, lengths must match exactly, and `ml_dsa65.verify(pq_pubkey,
  msg, pq_signature)` must succeed. Otherwise the transaction is invalid.
- A mismatching but well-formed co-signature makes the transaction invalid rather
  than merely ignored, so a stripped or replaced quantum half cannot be smuggled in.

### Phase B — account binding (height `PQ_BIND_HEIGHT`)

Adds a transaction kind `pq_bind` carrying `pq_pubkey` and both signatures. Account
records gain `pq_pubkey: Option<[u8; 1952]>` and `pq_required: bool`, both included
in the account's state hash.

For `height >= PQ_BIND_HEIGHT`, if an account has `pq_required = true`, any
transaction from it without a valid co-signature over the bound key is invalid.
Binding is one-way; rebinding requires a valid co-signature from the currently bound
key.

### Phase C — mandatory (height `PQ_REQUIRE_HEIGHT`)

For `height >= PQ_REQUIRE_HEIGHT`, every transaction must carry a valid ML-DSA-65
co-signature. Accounts with no bound key bind implicitly on their first
post-activation transaction.

Sizing changes required before Phase C: per-transaction size cap raised to admit a
~5.4 KB transaction, mempool byte-budget and per-account queue limits recalculated,
and gas metering updated so the larger verification and bandwidth cost is priced.

## Rationale

Splitting into three heights separates cheap safety (reject forged quantum halves)
from expensive changes (block-space sizing). Binding exists so security-conscious
holders and contracts can opt in early rather than waiting for the network-wide
switch.

ML-DSA-65 is used rather than the higher parameter set because the extra bytes buy
margin against no attack that the middle set is known to fail; hash-based schemes
(SLH-DSA) were rejected for signature size and signing cost.

## Backwards compatibility

Phases A and B are backwards compatible for transactions that omit the fields.
Phase C is a consensus break for classical-only wallets, so it needs a long lead
time and coordinated SDK, wallet and explorer releases. Blocks before each height
keep validating under the old rules.

## Security considerations

- **Downgrade attacks:** rejecting invalid-but-present co-signatures in Phase A
  prevents an intermediary rewriting a dual-signed transaction into a classical one.
- **Denial of service:** ML-DSA verification is heavier than Ed25519, so Phase A must
  land only with the existing parallel verification path and per-account throttles,
  and Phase C must reprice gas or an attacker buys CPU cheaply.
- **Key derivation:** both keys come from one master secret with domain separation, so
  a break in one derivation must not leak the other; the labels are fixed and must
  never be reused for a third purpose.
- **Legacy keys:** raw-hex imported accounts have no quantum half and must be told,
  in the wallet, to migrate to a freshly derived account.

## Test plan

Known-answer vectors from FIPS 204, replay of historical blocks across each height,
devnet runs with mixed classical and dual-signed traffic, and load tests measuring
throughput at full 5.4 KB transactions before Phase C is scheduled.

## Reference implementation

None yet — wallet-side derivation and signing are already live.

## Copyright

Released into the public domain.
