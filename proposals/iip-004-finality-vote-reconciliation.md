# IIP-4: Finality Vote Reconciliation

**Status:** Implemented (devnet) · **Layer:** Consensus

Finality requires strictly more than 2/3 of validator weight. In a 3-validator set that means ALL three votes — a single offline validator stalls finality. To recover quickly when a node rejoins, nodes exchange `getvotes`/`sync_votes` P2P messages to reconcile vote sets instead of waiting for the next round.

Reproducible with `devnet.sh 3`; regression-covered in the conformance suite.
