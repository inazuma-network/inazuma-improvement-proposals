# IIP-2: Slashing & Jailing

**Status:** Implemented (devnet) · **Layer:** Consensus

Validators that miss slots or equivocate are penalized in-protocol. Missed-slot jailing activates at height 130,000; jailed validators are excluded from the active set until a fixed unjail height, and may self-unjail afterwards. Double-signing is slashed by burning a share of stake.

A guard prevents a validator from signing two different blocks at the same height even if the operator restarts with stale state. Parameters and CLI/RPC reporting: `docs/validator/slashing.md` in inazuma-docs.
