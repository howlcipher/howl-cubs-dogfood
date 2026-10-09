# HOWL-CUBS HANDOFF (recovery entry point)

Updated: 2026-10-09T19:58Z

## Mission and state
- Mission: discover and test where software could create a meaningful baseball advantage for the Chicago Cubs, while dogfooding the Howl ecosystem (HowlDream discovery, HowlPlane lifecycle).
- Campaign status: ACTIVE. Mode: USER MODE. Phase: R001 CLOSED (COMPLETE_NEGATIVE, merged); next R002 REGRESSION under prompt v2 (CLOSEOUT-PROMPT.md part B).
- Run: R001, type DISCOVERY_BUILD. Prompt: EXECUTION-PROMPT.md v1 sha256 e1b5ca33…a5439d6. Snapshot: runs/R001/PROMPT-SNAPSHOT.md. Contract: runs/R001/RUN-CONTRACT.md.
- Selected opportunity: MILBFA (November free-agent pool triage). Design: experiments/R001-MILBFA-DESIGN.md (v1 kept as .v1.md; v2 + pre-holdout k=50 + exploratory addendum). Result: pre-registered early stop (validation primary positives 23 < 30); exploratory whole pool: model ties prior-MLB-time rule. Proposed outcome COMPLETE_NEGATIVE. 2025 holdout untouched.

## Versions
- howlplane main 4b7fe81 (engine src == bb7ba21). howldream main f1cc5ca. howl c9d37d5 (binary built 2026-10-07; version reports commit unknown). Full table: REPOSITORIES.json / REPOSITORIES.md.
- Baseball project: cubs-edge-lab main 7c715c8.

## Findings and repairs
- DOG-035..039 FIXED and merged (howlplane f39cf18, 28511b4, af9f40a; howl 45478f4); howlplane registry text for DOG-037/038/039 still says FIX IN REVIEW. Open: CUBS-P-002, CUBS-P-003, CUBS-P-008, NOTE-008. See DOGFOOD.md.

## Git / publication
- Control repo: PUBLIC https://github.com/howlcipher/howl-cubs-dogfood; main 5d9235d; R001 records on branch `records/R001` (pushed, no PR yet).
- Baseball repo: PUBLIC https://github.com/howlcipher/cubs-edge-lab; main 7c715c8 (scaffold only). Baseball project SHA: 7c715c8.
- Howl repos: unmodified. PR/check/merge state: none.

## Research and validation
- Data cutoff: not yet set. Holdout: not yet defined.

## Last successful action
- Dream discovery complete: pass 1 (hd-20261008-143010-528e5330b08f, PARTIAL by operator budget), pass 2 critique (hd-20261008-143634-565a00716635), pass 4 second divergent (hd-20261008-144047-5e3c07475c85). Finalists in OPPORTUNITIES.md. cubs-edge-lab created, pushed (main 7c715c8), factory prepared.

## Next action
- Start R002 (REGRESSION) per CLOSEOUT-PROMPT.md part B under EXECUTION-PROMPT v2: write runs/R002/RUN-CONTRACT.md and snapshot the prompt, then repair CUBS-P-002, CUBS-P-003, CUBS-P-008, NOTE-008 and update DOG-037/038/039 registry status.
- cubs-edge-lab main f7caca6; howlplane main af9f40a (live engine); howl main 45478f4.

## Active processes
- None.

## Bounds remaining (R001)
- Dream passes 4 of 5; Dream calls 25 of 40; pivot 1 unused; HowlPlane sessions 14 of 16 (user-raised bound).

## Warnings / blockers
- Shared engine: `dev/howlplane` checkout is used by other campaigns; re-verify HEAD and branch before every orchestrate.
- Other Claude/Codex sessions run on this host; do not touch their worktrees.
- Pending help request: none.

LOCAL_LLM_USED: NO (22 Dream calls remote Claude; HowlPlane workers Claude, Codex gpt-6-astra, Cursor, AGY: all remote vendor CLIs per session ledger)
