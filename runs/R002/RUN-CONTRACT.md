# R002 run contract

- Run ID: R002. Type: REGRESSION (no baseball claim).
- Prompt: EXECUTION-PROMPT v2, sha256 7fa1b0c151ef593e07a4d3a3c44d1626a1ceae7693ca615a1d756f83b59d75d5 (snapshot: PROMPT-SNAPSHOT.md). Section 19A applies from this run.
- Question: can the open Howl findings from R001 be repaired and proven through the public workflow?
- Scope:
  1. CUBS-P-002: a refused test or verification command in an implementation attempt that changed nothing marks a proven agent interactive-only in the global readiness cache.
  2. CUBS-P-003: acceptance (and review) are shown a superseded planner VERIFY_COMMAND as the session's verification command.
  3. CUBS-P-008: an implementer's "IMPLEMENTATION_STATUS: INCOMPLETE" is recorded as SUCCEEDED and sent to review.
  4. NOTE-008: a documented usage refusal (`--separate` on a worktree with an unfinished session) is reported as INTERNAL_ERROR.
  5. Registry hygiene: DOG-037/038/039 marked FIXED in howlplane dogfood/findings/FINDINGS.md.
- Out of scope this run: acceptance applying criteria outside the goal (observed twice in R001; candidate for a later run after design discussion), NOTE-005/006/007.
- Reused evidence: R001 incident evidence (workflow-evidence/R001/), as reproduction input only.
- Acceptance criteria per finding: registry ID allocated after a collision check; isolated worktree from origin/main; root cause recorded; regression tests that fail without the fix; full pre-push gate; independent review through HowlPlane (existing-WIP, `--verify make test-full --verify-timeout` >= 2x last gate runtime); PR, CI green, merge; shared engine fast-forwarded; post-merge public proof; registry updated.
- Bounds: 6 HowlPlane sessions; 2 materially different attempts per root cause; Dream not needed. No new spending.
- Outcome labels: COMPLETE_REGRESSION if every in-scope item is merged and publicly proven; otherwise STOPPED_* with reasons.
