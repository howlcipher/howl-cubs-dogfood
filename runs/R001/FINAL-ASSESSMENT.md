# R001 final assessment

- Run: R001, DISCOVERY_BUILD. Prompt: EXECUTION-PROMPT v1 (sha256 e1b5ca33…a5439d6). Dates: 2026-10-08 to 2026-10-09.
- **Outcome: COMPLETE_NEGATIVE.** Required discovery, research, build and evaluation were completed; the pre-registered test stopped at its early-stop rule and the evidence does not support the claimed usefulness.
- Delivery: cubs-edge-lab MERGED_VERIFIED (main f7caca6); howlplane MERGED_VERIFIED (f39cf18, 28511b4, af9f40a); howl MERGED_VERIFIED (45478f4); howl-cubs-dogfood records: merged at closeout (see journal receipt). Campaign state: ACTIVE.

## Baseball

**Problem discovered.** HowlDream (passes 1, 2, 4; critique and blind-spot pass) moved discovery away from analyst dashboards toward offseason decisions. Minor-league free agency recurred in 4 of 6 second-pass trials: every November several hundred players elect free agency and clubs decide quickly, with limited scouting attention, whom to sign or invite.

**Alternatives and selection.** S1 measured public-data feasibility for three finalists. Rule 5 was rejected (explicit codes only from 2024). Call-up translation was rejected for this run (in-season, survivorship bias, strong public projections). MILBFA was selected: measurable pools (585-908 per offseason), public next-season outcomes, a named decision owner. Prior Cubs experiments (repertoire alerts, Wrigley wind, ABS) were excluded as explored families.

**Software built** (cubs-edge-lab, all through HowlPlane): a rate-limited Stats API probe with a content-free manifest; cohort construction with decision-time features (2018-2025 cohorts); outcome labels; baselines B0 (prior MLB time), B1 (level + rate stat), B2 (3-feature logistic), P (persistence) and model M; rolling-origin and validation evaluation with paired bootstrap; a pre-registration guard on the 2025 holdout; an exploratory whole-pool evaluation; a descriptive Cubs case. Bulk responses stay local under MLBAM's non-bulk terms.

**Design and evaluation.** Design v1 was written before any outcome label; the HowlDream challenge (pass 5) led to v2: stronger baselines, primary test in the low-attention segment (no MLB appearance in season Y), bootstrap and rolling-origin success rule, k = 50 fixed from measured Cubs signing volume (median 41) before any outcome, and an early-stop rule.

**Results.**
- Primary (pre-registered): early stop. In the no-MLB-in-Y segment, players reaching >=50 MLB PA or >=20 IP the next season: 2018 20, 2019 12, 2021 14, 2022 12, 2023 22, 2024 (validation) 23, below the minimum of 30. No model was fitted for the primary test; the 2025 holdout is untouched. Independently recounted from raw season files.
- Exploratory (whole pool, user-requested, no claim): model M roughly ties B0, the "played in MLB last year" ranking: top-50 hits 43/41, 48/44, 41/43, 38/32 (2021-2024); 2024 AUROC difference -0.016 (95% -0.049 to 0.014).
- Practical significance: the low-attention pool holds about 12-23 next-season contributors per year league-wide, under one per club. A ranking tool has little hidden value to find there, and where value is larger (players with MLB time) the obvious rule already finds it.

**Cubs application (dated, descriptive).** November-pool players the Cubs signed: 2021 14, 2022 12, 2023 10, 2024 8; of these, 3, 1, 6 and 0 reached the threshold next season. In the no-MLB-in-Y segment the Cubs had 1, 0, 1, 0 of 14, 12, 22, 23 league-wide positives. This establishes only the size of the opportunity: even perfect selection in that segment would have added roughly one player per year. It does not evaluate the Cubs' decisions (availability, terms, competing offers, roster needs are unknown).

**Limitations.** The outcome measures playing time, which reflects opportunity as well as talent; no public value metric (WAR) in this source; no comparison with projection systems (not lawfully bulk-accessible); thin samples per segment; two methods to separate minor-league from MLB elections disagree (~15%), sidestepped by using the whole pool.

**Repeated practical use:** unproven. **Rejected ideas** and evidence: OPPORTUNITIES.md. **Strongest next opportunity:** a FOLLOW_UP on MILBFA only with a genuinely new feature hypothesis (the 2025 holdout remains available for one pre-registered confirmatory test), or a new discovery using the unused pivot.

## Howl

**Components used.** HowlDream: 6 remote exploration passes (divergent, critique, second divergent, selection challenge, three prompt-review rounds; 34 of 40 calls; all remote Claude, mocked=false). HowlPlane: 15 orchestration sessions on cubs-edge-lab and on howlplane repair worktrees (planning, implementation, verification, independent review, rework, acceptance). Codex, Claude, Cursor and AGY all served as workers; Devin did not. HowlCreate, HowlWriter, HowlFrame, HowlProof, HowlForge: not used (no distinct role for them in this workload).

**Integration strengths.** Falsifying reviews caught real defects every time (stale reports, unsupported claims, an incomplete pipeline, missing planning coverage); bounded rework and HANDOFF states stopped bad work from merging; failover handled timeouts, quota limits and permission denials.

**Findings and repairs (all merged with regression tests, HowlPlane review and post-merge public proof).**
- DOG-035: `--verify` rejected commands with flags, including howl's README example (howlplane #164, howl #16).
- DOG-036: fixed, undocumented 300 s verification timeout (#164, `--verify-timeout`).
- DOG-037: AGY's read-only roles were not enforced; a reviewer rewrote research files and a verified implementation was lost (#165).
- DOG-038: no supported way to give workers network access for public data (#165, `--worker-network`).
- DOG-039: a Codex usage limit was recorded as "not authenticated" (#166).

**Open.** CUBS-P-002 (a refused test command marks a proven agent interactive-only everywhere); CUBS-P-003 (acceptance shown a superseded planner VERIFY_COMMAND); CUBS-P-008 (declared INCOMPLETE recorded as success); NOTE-005 (Dream validate does not warn on budget sizing); NOTE-006 (exported units carry pooled assumptions); NOTE-007 (flaky hygiene test); NOTE-008 (usage refusal reported as INTERNAL_ERROR); acceptance applying criteria outside the goal (observed twice). External: Claude and Codex session/usage limits (handled by failover).

**Local-LLM compliance.** LOCAL_LLM_USED: NO. HOWL_FORBID_LOCAL_INFERENCE=1 on every invocation; all Dream calls remote (manifests); all HowlPlane workers remote vendor CLIs.

**Limits on autonomy claims.** Several steps needed the user: repository visibility, three repair authorizations, a data-terms decision, two session-bound increases, a history rewrite, and two acceptance decisions. Controller interventions were outside product code: goals, constraints, discards, audits and merges.

## Durability

- Reproduction: cubs-edge-lab README (`python3 -m cubs_edge_lab.probe`, the triage CLI commands); raw data is re-created locally for individual use.
- Evidence: workflow-evidence/R001/ (dream, plane, repair-DOG-035, repair-DOG-037-038, repair-DOG-039); bulk data only in ignored private/ directories.
- Repositories at assessment: howlplane main af9f40a; howl main 45478f4; howldream main f1cc5ca (unchanged); cubs-edge-lab main f7caca6 (PRs #1-#5); howl-cubs-dogfood records/R001 (merged at closeout).
- Data incident: bulk MLB records were pushed to records/R001 and removed by an authorized history rewrite (d7fa0f9); GitHub may still serve orphaned commits by SHA until a Support purge by the owner.
- Prompt: v1 used. v2 (section 19A, sha256 7fa1b0c1…) adopted for R002 after three HowlDream review rounds.
- Help requests (user answers in the journal): repo visibility; DOG-035 repair; DOG-035 acceptance gate; S1 blockers; S1 exhausted rework + CUBS-P-007; history rewrite; data terms; S2 block; exploratory scope; S3 acceptance.
- Next run: R002 REGRESSION (CLOSEOUT-PROMPT.md part B).
