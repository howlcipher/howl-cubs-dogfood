# R003 final assessment

- Run: R003, DISCOVERY_BUILD. Prompt: EXECUTION-PROMPT v2 (sha256 7fa1b0c1…; section 19A in effect).
- **Outcome: COMPLETE_NEGATIVE.** Public decision-time data does not let software find third-base holds that should have been sends. The pre-registered model does not beat a constant send-success rate on the 2026 holdout.
- Delivery: cubs-edge-lab PR_OPEN. PRs #7 (retrieval tool), #8 (table) and #9 (fit and holdout), stacked, are SSH-signed and HowlPlane-accepted. Self-merge was blocked by the Claude Code permission classifier, so they await the user's merge. #6 (feasibility) is merged (537af76). howl-cubs-dogfood records are on records/R003. howlplane, howl and howldream are NOT_APPLICABLE (unchanged).

## Discovery

- HowlDream passes 1, 2 and 4, plus a design challenge, used 27 of 40 calls. All ran on remote Claude, not mocked. LOCAL_LLM_USED: NO.
- The pass-2 critique found that the first-pass candidates (qualifying offers, posting, arbitration, coach moves, postseason rosters) have too few cases to test. A critique-informed pass then targeted high-frequency decisions. SENDHOLD (third-base send/hold) was selected: it has thousands of cases per season, and its outcome and decision-time inputs are public.

## Experiment

| Step | Evidence |
| --- | --- |
| Feasibility: 100 games, 107 requests | PARTIAL. Outcomes and person-ID joins are observable; the counterfactual for holds is not (PR #6) |
| Design v1, then v2 after the HowlDream challenge, frozen before retrieval | experiments/R003-SENDHOLD-DESIGN.v1.md and v2.md |
| Full retrieval: 2,430 of 2,430 games per season, 4,868 of the 4,900 authorized requests, 0 failures | workflow-evidence/R003/R3-retrieval.log |
| v3 amendment, approved by the owner before any model fit | The feed splits one continuous advance at third base, and about 94% of v2 "AMBIGUOUS" runners scored on the hit itself (design v3 amendment table) |
| 2025 fit, frozen (sha256 b463af22…), then the 2026 evaluation run once | research/SENDHOLD_EXPERIMENT.md (PR #9) |

### Results (2026 holdout, v3 primary)

- Sends: 1,927. Out at home: 71. The 2025 out rate was 3.6% (67 of 1,840).
- Not enough outs for the full covariate set (67 against the 140 required), so the reduced model was used, as pre-registered.
- Brier score: model 0.0354, constant 0.0355, difference −0.0001, 95% interval [−0.0003, 0.0005]. This fails the pre-registered gate, so the verdict is **NEGATIVE**.
- The pre-registered v2 definition is also **NEGATIVE** (difference −0.0015, interval [−0.0035, 0.0018]). So are both sensitivity analyses: AMBIGUOUS counted as SENT_SAFE, and fallback rows dropped.
- The "runs left" estimate (327 runs in 2026) is **not interpretable**. Its interval excludes zero, but with a flat model it only restates that about 96% of sends succeed, extrapolated to holds, which is the selection problem the design warned about. The Brier gate exists to block exactly this, and it did.
- Cubs (descriptive): 157 and 159 opportunities in 2025 and 2026. Their flagged holds are subject to the same caveat.

### Interpretation

- INFERENCE: coaches already send almost only runners who will be safe (about 96% success). Season-level speed, arm strength, hit zone and outs do not separate the failures.
- What would decide whether more sends are warranted is play-level tracking: runner position and speed at the fielder's pickup, and throw distance and accuracy. Those are not in the public feed.
- A public-data send/hold tool would not give the Cubs a measurable edge.

## Howl observations

- HowlPlane sessions: 5 of 10 (bede6ab6, 5c97be4f, dead6ab8, 0dd5cc50, 16cb9bab). All ended accepted with audit CLEAN.
- Failover worked live every time:
  - DOG-042 IMPLEMENTATION_INCOMPLETE: Cursor and Codex rerouted with partial work kept.
  - DOG-040 session-scoped permission exclusions: agents doctor showed all workers READY after every session.
  - DOG-043: a usage refusal, not INTERNAL_ERROR.
- New: NOTE-009. The 1800 s worker cap prevents long, rate-limited data pulls inside a worker. Workaround: build the tool in a session, run the merged tool outside, then process offline.
- Review effectiveness: the R3-S1 independent review missed per-segment double counting, which the controller audit caught. Then the controller's own acceptance targets for R3-S2 were wrong. They came from the defective artifact; the worker refused to force a match, and the controller recount from raw data confirmed the worker.
- Pre-registration: the AMBIGUOUS label rested on a sample-level reading of the feed and was corrected before fitting, with owner approval.

## Prompt retrospective

Prompt v3 adopted after HowlDream review (workflow-evidence/R003/dream/p6-prompt-v3-review-texts.txt; reconciliation in the journal). Final diff: workflow-evidence/R003/prompt-v3.final.diff. Lessons:

1. Controller acceptance numbers come from raw data, never from the artifact being corrected (R3-S2).
2. Before freezing labels, check every label rule against the event structure of the full retrieved data, not only a sample. Count how the source encodes each case (R3-S4).
3. Plan long data pulls as a separate, merged tool run outside the worker (NOTE-009).
4. Take journal heading times from `date -u` (carried over from R002).
