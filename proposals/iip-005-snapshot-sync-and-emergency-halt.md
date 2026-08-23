# IIP-5: Snapshot Sync & Emergency Halt

**Status:** Implemented (devnet) · **Layer:** Consensus

Format-2 state snapshots let a fresh node join without replaying history: snapshot tables are dumped at a finalized height, verified against the state root, then sync proceeds from that point. An emergency halt lets the validator set pause block production on detection of a critical fault, with an explicit on-chain resume.
