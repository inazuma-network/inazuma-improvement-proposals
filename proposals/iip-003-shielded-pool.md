# IIP-3: Shielded Pool

**Status:** Implemented (devnet) · **Layer:** Consensus

A Zcash-style shielded pool using Groth16 proofs over a Poseidon-hashed note Merkle tree. Three transaction kinds — Shield, PrivateTransfer, Unshield — are consensus-gated behind an activation height. Nullifiers prevent double-spends; the pool root is committed into the V2 state root, and the V1 state root includes a pool digest so pre-activation nodes cannot silently fork.

Includes SNARK circuits, Merkle membership proofs, and a conformance suite covering activation gating, double-spend rejection and shield→unshield round trips.
