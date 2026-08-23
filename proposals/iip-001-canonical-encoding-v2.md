# IIP-1: Canonical Transaction Encoding V2

**Status:** Implemented (devnet) · **Layer:** Consensus

Every consensus-critical hash (transactions, blocks, votes) is computed over a single canonical byte encoding: fixed field order, explicit length prefixes, no map ordering dependence. V1 hashes remain valid for historical blocks; V2 activates at a scheduled height so old nodes never disagree about history.

Motivation: ambiguous encodings are a consensus-split vector — two nodes hashing the same logical transaction differently fork silently. V2 removes that class of bug and is enforced by the conformance suite.
