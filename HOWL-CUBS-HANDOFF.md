# HOWL-CUBS HANDOFF (recovery entry point)

Updated: 2026-10-08T19:15Z

## Mission and state
- Mission: discover and test where software could create a meaningful baseball advantage for the Chicago Cubs, while dogfooding the Howl ecosystem (HowlDream discovery, HowlPlane lifecycle).
- Campaign status: ACTIVE (controller stopped at the usage limit mid-repair). Mode: REPAIR MODE (DOG-035/036). Phase: R001 pre-selection; S1 feasibility research waits for the DOG-035 merge.
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
1. Check review session 2: `cat workflow-evidence/R001/repair-DOG-035/09-review2.exit` and `09-review2.stdout`; `PYTHONPATH=/run/media/system/tallgeese/dev/howlplane-dog035/src howl orchestrate inspect --repo /run/media/system/tallgeese/dev/howlplane-dog035-review2`. It runs on candidate engine 1a5d783 with `--verify make test-full --verify-timeout 1500`.
2. If COMPLETE: fold any worker changes from the review2 worktree into branch dogfood/DOG-035-verify-argv (attributed commit), wait for PR #164 CI, merge #164, then howl #16; ff-only the shared dev/howlplane checkout; verify the engine provenance; replay S1 via the public CLI with `--verify "python3 -m pytest -q"`.
3. If HANDOFF/BLOCKED: preserve the ledger (sanitized, like 06-session-ledger.md) and pause for help.
4. Queue the follow-up repairs CUBS-P-002 (false interactive-only downgrade) and CUBS-P-003 (stale planner verify shown to acceptors), as authorized 17:55Z.
- Check `howl agents doctor` for readiness downgrades after every session (Claude hit SESSION_LIMIT in session 2).

## Active processes
- Review session 2 (`howl orchestrate`, background shell of the ended controller session), started 2026-10-08T17:57Z local 12:57. It may still be running or finished; check the exit file. Do not launch a duplicate.

## Bounds remaining (R001)
- Dream passes: 3 of 5. Dream calls 22 of 40. Pivots 1 unused. HowlPlane sessions 2 of 8. DOG-035 repair attempts: 2 of 2 (fe0ff37+aae9545, then 1a5d783/DOG-036): a further failure requires a help request.

## Warnings / blockers
- Shared engine: `dev/howlplane` checkout is used by other campaigns; re-verify HEAD and branch before every orchestrate.
- Other Claude/Codex sessions run on this host; do not touch their worktrees.
- Pending help request: DOG-035 acceptance gate and CUBS-P-002..004 (asked 2026-10-08T17:50Z).

LOCAL_LLM_USED: NO (22 Dream calls remote Claude; HowlPlane workers Claude, Codex gpt-6-astra, Cursor, AGY: all remote vendor CLIs per session ledger)
