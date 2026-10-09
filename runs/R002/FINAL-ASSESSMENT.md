# R002 final assessment

- Run: R002, REGRESSION. Prompt: EXECUTION-PROMPT v2 (sha256 7fa1b0c1…; section 19A in effect).
- **Outcome: COMPLETE_REGRESSION.** No baseball claim (not reassessed this run).
- Delivery: howlplane MERGED_VERIFIED (#167 -> 99d3aa4); howl-cubs-dogfood records merged at closeout. cubs-edge-lab, howl, howldream: NOT_APPLICABLE (unchanged).

## Scope and results

| Finding | Was | Fix | Proof |
| --- | --- | --- | --- |
| DOG-040 (CUBS-P-002) | A refused Bash command during no-edit validation marked a proven agent interactive-only everywhere | Named Bash-only refusals in a mutating role are a session-scoped grant gap regardless of edits; edit-tool, unnamed and mixed (unnamed_denials) denials still count | Live: proof session 55b942cf reproduced the exact scenario (Claude refused `python -m unittest discover...`, no edits); excluded for the session only, rerouted to Codex, session COMPLETE, `agents doctor` Claude READY after |
| DOG-041 (CUBS-P-003) | Acceptance cited a superseded planner VERIFY_COMMAND | Review and acceptance told which command verifies the session | Live condition occurred (planned and explicit commands differed); no misattribution; inclusion covered by contract tests |
| DOG-042 (CUBS-P-008) | Declared INCOMPLETE recorded as success | IMPLEMENTATION_INCOMPLETE role failure; partial changes and reason kept; handoff if no implementer can finish | Contract tests (end-to-end reroute and handoff); not observed live (cannot be triggered without staging a failure) |
| DOG-043 (NOTE-008) | Correctable refusals printed "likely a HowlPlane bug ... INTERNAL_ERROR" | OrchestrateRequestError -> ORCHESTRATE_REQUEST_REFUSED with next step for 21 user-correctable sites; internal failures unchanged | Live CLI on merged main: `resume` with no session and `--verify-timeout 0` render the operator error |
| Registry | DOG-037/038/039 said FIX IN REVIEW | Marked FIXED | Merged in #167 |

Independent review: HowlPlane existing-WIP session (engine af9f40a, `--verify make test-full --verify-timeout 1000`): COMPLETE, audit CLEAN; its worker tightened three fixes (status-line regex, unnamed denials, internal session-ID error), folded in as 022d294. Pre-push gate 2455 passed; CI green; PR head equal to the accepted tree.

## Howl observations

- Environment: the first pre-push run hit `flake8 MemoryError: Parser stack overflowed` after all tests passed, during a controller process restart; not reproducible; one bounded retry passed. Recorded as an environment limitation, not a Howl defect.
- Open (not in scope): acceptance applying criteria outside the goal (observed twice in R001); NOTE-005 (Dream validate budget warning); NOTE-006 (pooled assumptions in Dream unit exports); NOTE-007 (flaky hygiene test).
- HowlPlane sessions: 2 of 6. LOCAL_LLM_USED: NO.

## Prompt review

No prompt change from R002. Candidate for the next revision (needs review): take every journal heading time from `date -u` (R001 correction entry). Not adopted yet.
