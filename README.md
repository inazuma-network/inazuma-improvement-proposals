# Inazuma Improvement Proposals (INAZIPs)

An INAZIP is a written proposal to change how Inazuma works. If a change affects
consensus, fees, staking, slashing, transaction encoding, cryptography, or anything
every node must agree on, it starts here — before any code is merged.

## Why this exists

Nodes only stay on the same chain because they follow identical rules. So rule
changes need to be written down, argued about in public, implemented behind an
activation height, and adopted by validators on purpose. This repository is the
written-down part.

## Do I need an INAZIP?

| Change | INAZIP? |
| --- | --- |
| Bug fix, refactor, docs, tooling | No — just open a pull request in the right repo |
| New RPC method or SDK helper | No — issue and pull request is enough |
| New transaction type or field | Yes |
| Fee, reward, or slashing parameters | Yes |
| New precompile or cryptographic primitive | Yes |
| Change to block format, state layout, or address format | Yes |
| Anything that makes old nodes reject new blocks | Yes, always |

If you are unsure, open a draft. Nobody minds.

## How to submit

1. Read [process.md](process.md) once. It is short.
2. Copy [template.md](template.md) to `proposals/inazip-draft-<short-title>.md`.
3. Fill it in. The **Specification** section must be precise enough that two people
   could implement it separately and get the same bytes.
4. Open a pull request. Discussion happens in that pull request.
5. An editor assigns a number when the proposal is complete enough to review.

You do not need to be a Rust developer, and you do not need working code to open a
draft. A clear problem statement with a concrete rule change is a valid proposal.

## Life of a proposal

```text
Draft  →  Review  →  Accepted  →  Scheduled  →  Live
                 ↘  Rejected / Withdrawn
```

| Status | Meaning |
| --- | --- |
| Draft | Written, not yet reviewed in depth |
| Review | Actively discussed; specification being tightened |
| Accepted | Rules agreed; implementation can begin |
| Scheduled | Implemented, released, with an activation block set |
| Live | Activation block passed; this is now how the chain works |
| Rejected | Declined, with the reason recorded in the file |
| Withdrawn | Author stopped pursuing it |

Nothing is Live because a pull request merged. It is Live because validators ran the
release and the chain passed the activation block.

## Index

| Number | Title | Status |
| --- | --- | --- |
| [1](proposals/inazip-1.md) | INAZIP purpose and process | Live |
| — | [Staged verification and enforcement of ML-DSA-65 co-signatures](proposals/inazip-draft-pq-enforcement.md) | Draft |

## Related repositories

- [inazuma-core](https://github.com/inazuma-network/inazuma-core) — node implementation
- [inazuma-docs](https://github.com/inazuma-network/inazuma-docs) — user-facing documentation
- [inazuma-sdk](https://github.com/inazuma-network/inazuma-sdk) — TypeScript SDK

## License

Proposals are released into the public domain unless a proposal states otherwise.
