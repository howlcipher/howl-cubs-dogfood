# R001 run contract

- Run ID: R001 (campaign run IDs are separate from DOG IDs and from the household campaign's run-NNN)
- Type: DISCOVERY_BUILD
- Prompt: EXECUTION-PROMPT.md v1, sha256 e1b5ca33c7f1d85c5b8b2145be2133ec50e811feaf13e9a97bc3a5fcda5439d6 (snapshot: PROMPT-SNAPSHOT.md)
- Execution date: 2026-10-08 (UTC). Baseball decision date: to be established from live public evidence, not assumed.
- Question: "Where could software create a meaningful baseball advantage for the Chicago Cubs?"
- Scope: broad Dream discovery, evidence-based selection, research, baseline, real implementation through HowlPlane, validation, a dated Cubs application, independent review, acceptance, usefulness assessment, and per-repository merge closeout.
- Reused evidence: none as evidence. Prior Cubs experiments (howl-creative-dogfood/run04) are only used as declared `explored_families` diversity memory; their conclusions are not carried forward.
- Acceptance criteria: the DISCOVERY_BUILD completion list in section 19, with an honest outcome label (COMPLETE_USEFUL / COMPLETE_NEGATIVE / STOPPED_*).

## Finite bounds (per run)

| Resource | Bound | Rationale |
| --- | --- | --- |
| Dream passes | 5 (divergent, cluster/critique, blind spot, second divergent, optional selection challenge) | Section 8 requires at least four distinct steps; one spare |
| Dream remote calls | 40 total across passes | Each pass is capped by `--max-calls`/budget.max_calls |
| Opportunities | 1 primary + at most 1 substantive pivot | Section 9 |
| HowlPlane sessions | 8 for the run (research, build, follow-on rework, reviews) | Each session already caps rework at 2 rounds |
| HowlPlane execution budget | default per role; raise to at most 1200 s only with recorded reason | Prefer decomposing goals |
| Repair attempts | 2 materially different attempts per root cause | Section 14 |
| Provider retries | Only those in configured public retry/failover policy | No manual re-shopping |
| Campaign-wide money | No new paid capacity. Existing CLI subscriptions only. | No user budget specified |

## Worker contract (passed via `--constraint` on every orchestrate)

"The current HowlPlane session IS the orchestration workflow. Perform the assigned work in the target project. Do not launch howl/howlplane or another orchestration session merely to prove Howl was used. Do not copy internal orchestration state into the target project."

## Local inference

HOWL_FORBID_LOCAL_INFERENCE=1 exported for every Howl/provider invocation. Dream budgets set `forbid_local_inference: true`, `allow_local_inference: false`.

## Amendment 2026-10-08T23:45Z (user-authorized)
- HowlPlane sessions bound for R001 raised from 8 to 12 by the user (help response on S1 exhausted rework + CUBS-P-007). Sessions already consumed (6) are not reset.
