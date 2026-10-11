# HOWL-CUBS HANDOFF (recovery entry point)

Updated: 2026-10-11 (campaign synthesis)

## Mission and state
- Mission: discover and test where software could create a meaningful baseball advantage for the Chicago Cubs, while dogfooding the Howl ecosystem (HowlDream for discovery and critique, HowlPlane for the build lifecycle).
- **Campaign status: PAUSED_NEEDS_HELP.** Mode: USER MODE.
  - R001 to R005 are closed and MERGED_VERIFIED.
  - The campaign synthesis is written (CAMPAIGN-SYNTHESIS.md). The question for the owner is in its section 5: stop, allow non-public data, run a MILBFA confirmatory test, run a COACH feasibility check, or do Howl-only regression work.
- Prompt: EXECUTION-PROMPT.md v4, sha256 048f2f5bcd32259298d625139e326da54ca45da9aed58d96719b395e3dff63b0. No change is proposed from the synthesis.

## Run index
See FINAL-ASSESSMENT.md (index) and runs/R00N/FINAL-ASSESSMENT.md.
- R001 COMPLETE_NEGATIVE.
- R002 COMPLETE_REGRESSION.
- R003 COMPLETE_NEGATIVE.
- R004 COMPLETE_USEFUL, bounded.
- R005 COMPLETE_USEFUL, bounded.

## Versions (REPOSITORIES.json / REPOSITORIES.md)

| Repository | State |
| --- | --- |
| cubs-edge-lab | main 97901d2 (public; no CI) |
| howlplane | main 04b5269; the engine venv imports the shared checkout |
| howl | main 45478f4 |
| howldream | main f1cc5ca (unchanged) |
| howl-cubs-dogfood | main 64e7969; synthesis on records/synthesis |

## Synthesis delivery

| Repository | Change | State |
| --- | --- | --- |
| howlplane | dogfood/findings/FINDINGS.md status lines for DOG-040 to DOG-044 (FIX IN REVIEW, now FIXED). Session 745c5055 COMPLETE; PR #169 merged as 04b5269 (CI green); shared checkout fast-forwarded; worktree removed | MERGED_VERIFIED |
| howl-cubs-dogfood | records/synthesis: CAMPAIGN-SYNTHESIS.md, R005 MERGED_VERIFIED, record corrections, CUBS-P-012, manifest refresh, this handoff | PR #7 to main. The merge commit on main is the delivery receipt; there is no CI, and the sweep for bulk data and secrets was clean |

## Open findings (DOGFOOD.md)
- CUBS-P-009: no loopback-only network grant.
- CUBS-P-011: exact-literal `--verify` grant; Claude refused ungranted test commands.
- CUBS-P-012: a nonexistent `--repo` path is reported as INTERNAL_ERROR.
- CUBS-P-013: a CLEAN review whose status line is not last is treated as findings.
- NOTE-005, 006, 007, 009 and 010.
- Acceptance applied criteria outside the goal (R001).
- Optional owner configuration: `extra_allowed_bash` for Claude.

## Resume procedure
1. Read this file, then the last entries of MISSION-JOURNAL.md.
2. Reconcile with Git (all five repositories) and `howl orchestrate inspect --json`.
3. Export HOWL_FORBID_LOCAL_INFERENCE=1 before any Howl or provider call.
4. Act on the owner's answer to CAMPAIGN-SYNTHESIS.md section 5. Do not start a run without it.

## Warnings
- Shared engine: the dev/howlplane checkout is used by other campaigns. Re-verify HEAD and branch before every orchestrate.
- Other Claude and Codex sessions run on this host. Do not touch their worktrees.
- Use absolute paths for `--repo` (CUBS-P-012 came from a relative-path typo).
- MLB data: bulk data stays in ignored data/ directories. Publish aggregates and at most 5 short examples, and sweep before every push.
- R001 data incident: orphaned commits with bulk records may still be served by SHA until the owner asks GitHub Support to purge them.

## Active processes
- None.

LOCAL_LLM_USED: NO (every run and the synthesis).
