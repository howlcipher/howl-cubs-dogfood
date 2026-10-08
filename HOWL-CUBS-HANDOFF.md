# HOWL-CUBS HANDOFF (recovery entry point)

Updated: 2026-10-08T15:58Z

## Mission and state
- Mission: discover and test where software could create a meaningful baseball advantage for the Chicago Cubs, while dogfooding the Howl ecosystem (HowlDream discovery, HowlPlane lifecycle).
- Campaign status: PAUSED_NEEDS_HELP. Operating mode: USER MODE. Phase: R001 pre-selection feasibility research (HowlPlane S1 could not start).
- Run: R001, type DISCOVERY_BUILD. Prompt: EXECUTION-PROMPT.md v1 sha256 e1b5ca33…a5439d6. Snapshot: runs/R001/PROMPT-SNAPSHOT.md. Contract: runs/R001/RUN-CONTRACT.md.
- Hypothesis: not yet selected. Opportunity: not yet selected. Project: not yet selected. Experiment: not yet selected.

## Versions
- howlplane main 4b7fe81 (engine src == bb7ba21). howldream main f1cc5ca. howl c9d37d5 (binary built 2026-10-07; version reports commit unknown). Full table: REPOSITORIES.json / REPOSITORIES.md.
- Baseball project: cubs-edge-lab main 7c715c8.

## Findings and repairs
- Active finding: CUBS-P-001 (provisional, expected DOG-035): `--verify` rejects commands containing flags (DOGFOOD.md). Owning repo: howlplane (+ howl README docs). Repair branch: none (awaiting user response).

## Git / publication
- Control repo: PUBLIC https://github.com/howlcipher/howl-cubs-dogfood; main 5d9235d; R001 records on branch `records/R001` (pushed, no PR yet).
- Baseball repo: PUBLIC https://github.com/howlcipher/cubs-edge-lab; main 7c715c8 (scaffold only). Baseball project SHA: 7c715c8.
- Howl repos: unmodified. PR/check/merge state: none.

## Research and validation
- Data cutoff: not yet set. Holdout: not yet defined.

## Last successful action
- Dream discovery complete: pass 1 (hd-20261008-143010-528e5330b08f, PARTIAL by operator budget), pass 2 critique (hd-20261008-143634-565a00716635), pass 4 second divergent (hd-20261008-144047-5e3c07475c85). Finalists in OPPORTUNITIES.md. cubs-edge-lab created, pushed (main 7c715c8), factory prepared.

## Next action
- WAIT for the user's answer to the pending help question (CUBS-P-001). Do not launch S1 or select a workaround before it.
- After the response: record it in the journal; if repair is authorized, run the section 14 loop in a howlplane worktree (branch e.g. dogfood/DOG-035-verify-argv), then replay S1 via the public CLI.
- Working directory: /run/media/system/tallgeese/dev/howl-cubs-dogfood
- Resume commands (verified): `export HOWL_FORBID_LOCAL_INFERENCE=1`; `howl orchestrate inspect --repo /run/media/system/tallgeese/dev/cubs-edge-lab` (expect: no sessions); S1 goal: workflow-evidence/R001/plane/S1-feasibility.goal.txt.

## Active processes
- None owned by this mission.

## Bounds remaining (R001)
- Dream passes: 3 of 5 used (one spare + selection challenge). Dream calls 22 of 40. Pivots 1 unused. HowlPlane sessions 0 of 8 started (S1 rejected at parse). Repair attempts for CUBS-P-001: 0 of 2.

## Warnings / blockers
- Shared engine: `dev/howlplane` checkout is used by other campaigns; re-verify HEAD and branch before every orchestrate.
- Other Claude/Codex sessions run on this host; do not touch their worktrees.
- Pending help request: CUBS-P-001 (asked 2026-10-08T15:58Z). Pending authority boundaries: none.

LOCAL_LLM_USED: NO (all 22 Dream calls remote claude-opus-5-5/claude-sonnet-5-5 per manifests; no HowlPlane worker has run)
