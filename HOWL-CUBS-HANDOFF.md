# HOWL-CUBS HANDOFF (recovery entry point)

Updated: 2026-10-09T04:50Z

## Mission and state
- Mission: discover and test where software could create a meaningful baseball advantage for the Chicago Cubs, while dogfooding the Howl ecosystem (HowlDream discovery, HowlPlane lifecycle).
- Campaign status: PAUSED_NEEDS_HELP. Mode: USER MODE. Phase: R001 pre-selection; S1c research COMPLETE (Howl) but deliverable must drop bulk MLB data before publication; public-branch history contains bulk data (decision pending).
- Run: R001, type DISCOVERY_BUILD. Prompt: EXECUTION-PROMPT.md v1 sha256 e1b5ca33…a5439d6. Snapshot: runs/R001/PROMPT-SNAPSHOT.md. Contract: runs/R001/RUN-CONTRACT.md.
- Hypothesis: not yet selected. Opportunity: not yet selected. Project: not yet selected. Experiment: not yet selected.

## Versions
- howlplane main 4b7fe81 (engine src == bb7ba21). howldream main f1cc5ca. howl c9d37d5 (binary built 2026-10-07; version reports commit unknown). Full table: REPOSITORIES.json / REPOSITORIES.md.
- Baseball project: cubs-edge-lab main 7c715c8.

## Findings and repairs
- DOG-035..039 FIXED and merged (howlplane f39cf18, 28511b4, af9f40a; howl 45478f4). Open: CUBS-P-002, CUBS-P-003 (queued). See DOGFOOD.md.

## Git / publication
- Control repo: PUBLIC https://github.com/howlcipher/howl-cubs-dogfood; main 5d9235d; R001 records on branch `records/R001` (pushed, no PR yet).
- Baseball repo: PUBLIC https://github.com/howlcipher/cubs-edge-lab; main 7c715c8 (scaffold only). Baseball project SHA: 7c715c8.
- Howl repos: unmodified. PR/check/merge state: none.

## Research and validation
- Data cutoff: not yet set. Holdout: not yet defined.

## Last successful action
- Dream discovery complete: pass 1 (hd-20261008-143010-528e5330b08f, PARTIAL by operator budget), pass 2 critique (hd-20261008-143634-565a00716635), pass 4 second divergent (hd-20261008-144047-5e3c07475c85). Finalists in OPPORTUNITIES.md. cubs-edge-lab created, pushed (main 7c715c8), factory prepared.

## Next action
- WAIT for the user's decision on records/R001 history (bulk MLB data in history; tip already clean at 49d8e17).
- Then: HowlPlane session S1d on cubs-edge-lab to keep bulk responses local (evidence and per-transaction observations ignored; publish code, manifest, aggregate counts, a few short examples), keep tests passing in a clean clone, and make FEASIBILITY.md human-readable. Then commit/PR/merge cubs-edge-lab and proceed to selection.
- cubs-edge-lab working tree: uncommitted S1c deliverable (COMPLETE session 2027b3fe). Do not commit research/evidence.json or research/observations.json as they are.

## Active processes
- None.

## Bounds remaining (R001)
- Dream passes 3 of 5; Dream calls 22 of 40; pivots 1 unused; HowlPlane sessions 8 of 12 (user-raised bound).

## Warnings / blockers
- Shared engine: `dev/howlplane` checkout is used by other campaigns; re-verify HEAD and branch before every orchestrate.
- Other Claude/Codex sessions run on this host; do not touch their worktrees.
- Pending help request: records/R001 history rewrite (asked 2026-10-09T04:50Z).

LOCAL_LLM_USED: NO (22 Dream calls remote Claude; HowlPlane workers Claude, Codex gpt-6-astra, Cursor, AGY: all remote vendor CLIs per session ledger)
