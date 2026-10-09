# Bulk third-party data removed from the public record

Two evidence files contained bulk MLB Stats API content (about 8,600 transaction records each). MLBAM's terms permit only "individual, non-commercial, non-bulk use" (gdx.mlb.com/components/copyright.txt), so they must not be published here. They were pushed in error to branch records/R001 and removed from the branch tip on 2026-10-09; they are kept locally under the ignored `private/` directory. SHA-256 of the removed files: BULK-DATA-REMOVED.sha256.

- `S1-blocked-worktree.patch`: the S1 attempt-1 working tree, including AGY's reviewer-written probe outputs (invalid as evidence).
- `S1-invalid-reviewer-outputs/probe_results.json`: AGY's reviewer-written probe output (invalid as evidence).

Reconstruction: the probe code and its manifest (endpoints, parameters, retrieval times, hashes) let anyone re-fetch the same public responses for individual use.
- `S1b-worktree.patch`: the S1 attempt-2 working tree (61 transaction records in its research outputs); moved for consistency.
