# HOWL-CUBS HANDOFF (recovery entry point)

Updated: 2026-10-08T17:50Z

## Mission and state
- Mission: discover and test where software could create a meaningful baseball advantage for the Chicago Cubs, while dogfooding the Howl ecosystem (HowlDream discovery, HowlPlane lifecycle).
- Campaign status: PAUSED_NEEDS_HELP. Operating mode: REPAIR MODE (DOG-035). Phase: R001 pre-selection; S1 feasibility research blocked on DOG-035 merge.
- Run: R001, type DISCOVERY_BUILD. Prompt: EXECUTION-PROMPT.md v1 sha256 e1b5ca33…a5439d6. Snapshot: runs/R001/PROMPT-SNAPSHOT.md. Contract: runs/R001/RUN-CONTRACT.md.
- Hypothesis: not yet selected. Opportunity: not yet selected. Project: not yet selected. Experiment: not yet selected.

## Versions
- howlplane main 4b7fe81 (engine src == bb7ba21). howldream main f1cc5ca. howl c9d37d5 (binary built 2026-10-07; version reports commit unknown). Full table: REPOSITORIES.json / REPOSITORIES.md.
- Baseball project: cubs-edge-lab main 7c715c8.

## Findings and repairs
- Active findings: DOG-035 (fix in PR howlplane #164 + howl #16; review session HANDOFF REQUIRED on acceptance gate). New: CUBS-P-002 (false interactive-only downgrade of Claude), CUBS-P-003 (acceptance cites superseded planner verify command), CUBS-P-004 (fixed 300 s verify timeout). See DOGFOOD.md.

## Git / publication
- Control repo: PUBLIC https://github.com/howlcipher/howl-cubs-dogfood; main 5d9235d; R001 records on branch `records/R001` (pushed, no PR yet).
- Baseball repo: PUBLIC https://github.com/howlcipher/cubs-edge-lab; main 7c715c8 (scaffold only). Baseball project SHA: 7c715c8.
- Howl repos: unmodified. PR/check/merge state: none.

## Research and validation
- Data cutoff: not yet set. Holdout: not yet defined.

## Last successful action
- Dream discovery complete: pass 1 (hd-20261008-143010-528e5330b08f, PARTIAL by operator budget), pass 2 critique (hd-20261008-143634-565a00716635), pass 4 second divergent (hd-20261008-144047-5e3c07475c85). Finalists in OPPORTUNITIES.md. cubs-edge-lab created, pushed (main 7c715c8), factory prepared.

## Next action
- WAIT for the user's answer to the help question of 2026-10-08T17:50Z (DOG-035 acceptance gate + CUBS-P-002..004). Do not merge, launch S1, or repair new findings before it.
- Resume commands: `export HOWL_FORBID_LOCAL_INFERENCE=1`; `howl orchestrate inspect --repo /run/media/system/tallgeese/dev/howlplane-dog035-review` (session 1426222b, HANDOFF REQUIRED, resumable); `gh pr checks 164 -R howlcipher/howlplane`; `howl agents doctor` (Claude interactive-only).
- Worktrees: dev/howlplane-dog035 (branch dogfood/DOG-035-verify-argv fe0ff37), dev/howlplane-dog035-review (origin/main + reviewed uncommitted diff), dev/howl-dog035 (docs/DOG-035-quote-verify a781bcb).

## Active processes
- None owned by this mission.

## Bounds remaining (R001)
- Dream passes: 3 of 5. Dream calls 22 of 40. Pivots 1 unused. HowlPlane sessions 1 of 8 (repair review 1426222b). DOG-035 repair attempts: 1 of 2 (fe0ff37 + reviewed worker improvements).

## Warnings / blockers
- Shared engine: `dev/howlplane` checkout is used by other campaigns; re-verify HEAD and branch before every orchestrate.
- Other Claude/Codex sessions run on this host; do not touch their worktrees.
- Pending help request: DOG-035 acceptance gate and CUBS-P-002..004 (asked 2026-10-08T17:50Z).

LOCAL_LLM_USED: NO (22 Dream calls remote Claude; HowlPlane workers Claude, Codex gpt-6-astra, Cursor, AGY: all remote vendor CLIs per session ledger)
