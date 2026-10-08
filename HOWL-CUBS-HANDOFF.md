# HOWL-CUBS HANDOFF (recovery entry point)

Updated: 2026-10-08T23:40Z

## Mission and state
- Mission: discover and test where software could create a meaningful baseball advantage for the Chicago Cubs, while dogfooding the Howl ecosystem (HowlDream discovery, HowlPlane lifecycle).
- Campaign status: PAUSED_NEEDS_HELP. Mode: USER MODE. Phase: R001 pre-selection; S1 feasibility research BLOCKED by review findings after 2 rework rounds (resumable); CUBS-P-007 open.
- Run: R001, type DISCOVERY_BUILD. Prompt: EXECUTION-PROMPT.md v1 sha256 e1b5ca33…a5439d6. Snapshot: runs/R001/PROMPT-SNAPSHOT.md. Contract: runs/R001/RUN-CONTRACT.md.
- Hypothesis: not yet selected. Opportunity: not yet selected. Project: not yet selected. Experiment: not yet selected.

## Versions
- howlplane main 4b7fe81 (engine src == bb7ba21). howldream main f1cc5ca. howl c9d37d5 (binary built 2026-10-07; version reports commit unknown). Full table: REPOSITORIES.json / REPOSITORIES.md.
- Baseball project: cubs-edge-lab main 7c715c8.

## Findings and repairs
- DOG-035/036/037/038 FIXED and merged (howlplane f39cf18, 28511b4; howl 45478f4). Open: CUBS-P-002, CUBS-P-003 (queued), CUBS-P-007 (usage limit recorded as not authenticated). See DOGFOOD.md.

## Git / publication
- Control repo: PUBLIC https://github.com/howlcipher/howl-cubs-dogfood; main 5d9235d; R001 records on branch `records/R001` (pushed, no PR yet).
- Baseball repo: PUBLIC https://github.com/howlcipher/cubs-edge-lab; main 7c715c8 (scaffold only). Baseball project SHA: 7c715c8.
- Howl repos: unmodified. PR/check/merge state: none.

## Research and validation
- Data cutoff: not yet set. Holdout: not yet defined.

## Last successful action
- Dream discovery complete: pass 1 (hd-20261008-143010-528e5330b08f, PARTIAL by operator budget), pass 2 critique (hd-20261008-143634-565a00716635), pass 4 second divergent (hd-20261008-144047-5e3c07475c85). Finalists in OPPORTUNITIES.md. cubs-edge-lab created, pushed (main 7c715c8), factory prepared.

## Next action
- WAIT for the user's answer to the help question of 2026-10-08T23:40Z (S1 exhausted rework + CUBS-P-007).
- cubs-edge-lab: uncommitted Cursor implementation from session 6713a592 (BLOCKED, resumable); tree archived as S1b-worktree.patch. Nothing committed beyond 7c715c8.
- Codex quota resets ~20:01 local (00:01Z); readiness cache wrongly says "not authenticated".
- Resume commands: `export HOWL_FORBID_LOCAL_INFERENCE=1`; `howl orchestrate inspect --repo /run/media/system/tallgeese/dev/cubs-edge-lab`; `howl agents doctor`.

## Active processes
- None.

## Bounds remaining (R001)
- Dream passes 3 of 5; Dream calls 22 of 40; pivots 1 unused; HowlPlane sessions 6 of 8; S1 attempts 3.

## Warnings / blockers
- Shared engine: `dev/howlplane` checkout is used by other campaigns; re-verify HEAD and branch before every orchestrate.
- Other Claude/Codex sessions run on this host; do not touch their worktrees.
- Pending help request: S1 exhausted rework + CUBS-P-007 (asked 2026-10-08T23:40Z).

LOCAL_LLM_USED: NO (22 Dream calls remote Claude; HowlPlane workers Claude, Codex gpt-6-astra, Cursor, AGY: all remote vendor CLIs per session ledger)
