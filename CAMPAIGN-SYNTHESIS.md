# Campaign synthesis: R001 to R005

- Question: "Where could software create a meaningful baseball advantage for the Chicago Cubs?"
- Period: 2026-10-08 to 2026-10-11 (MISSION-JOURNAL.md). Prompt versions v1 to v4 (EXECUTION-PROMPT.md; v4 sha256 048f2f5b…).
- Scope: this is a closeout synthesis of five closed runs. It is not a run and makes **no new baseball claim**.
- Sources: numbers come from the run assessments (runs/R00N/FINAL-ASSESSMENT.md) or from the files they cite: OPPORTUNITIES.md, MISSION-JOURNAL.md, DOGFOOD.md and EXECUTION-PROMPT.md. Where a number comes from one of those files, the file is named.
- Labels: FACT means measured in a run. INFERENCE means reasoned from evidence. SPECULATIVE means untested.
- Review: HowlDream reviewed two drafts (section 6). This version reconciles both rounds.
- **Campaign state: STOPPED_BY_USER** (2026-10-11). No next experiment with a ready hypothesis and more than small expected value was identified. Asked to choose from section 5, the owner chose "Stop the campaign".

## 1. Runs at a glance

| Run | Type | Prompt | Outcome | Baseball claim | Delivery |
| --- | --- | --- | --- | --- | --- |
| R001 MILBFA | DISCOVERY_BUILD | v1 | COMPLETE_NEGATIVE | No usefulness shown | MERGED_VERIFIED (cubs-edge-lab f7caca6; howlplane af9f40a; howl 45478f4) |
| R002 | REGRESSION (DOG-040 to 043) | v2 | COMPLETE_REGRESSION | None | MERGED_VERIFIED (howlplane 99d3aa4) |
| R003 SENDHOLD | DISCOVERY_BUILD | v2 | COMPLETE_NEGATIVE | No usefulness shown | MERGED_VERIFIED (cubs-edge-lab 91f06c5: #7 to #9 merged into their stacked parents, then #10 landed them on main; journal 2026-10-10T15:47:20Z) |
| R004 | FOLLOW_UP (results explorer) | v3 | COMPLETE_USEFUL, bounded | Presentation fidelity only | MERGED_VERIFIED (cubs-edge-lab 5fc04e9; howlplane 5253f3f) |
| R005 | FOLLOW_UP (explorer readability) | v4 | COMPLETE_USEFUL, bounded | Presentation only | MERGED_VERIFIED (cubs-edge-lab 97901d2) |

## 2. Answer to the question

**Short answer.** The campaign tested two public-data decision candidates. Neither produced evidence of an advantage:
- **MILBFA:** which November minor-league free agents to sign. Each year's pool holds 585 to 908 players, and the decision belongs to the pro scouting director.
- **SENDHOLD:** the third-base coach's send-or-hold call, with thousands of cases a season. On the 2026 holdout, the model from the pre-registered design (as amended in v3) did not beat a constant.
- **MILBFA:** the pre-registered test stopped at its early-stop rule before any model was fitted, because the target segment was too small. An exploratory whole-pool comparison roughly tied a simple rule.

This does not show that no public-data advantage exists anywhere. It shows that these two candidates did not yield one. The reasons differ:
- For SENDHOLD, INFERENCE (untested): the likely deciding input is play-level tracking, which is not in the Stats API feed used here. Other public sources were not assessed.
- For MILBFA, the opportunity was too small to test.

| Run | Result (FACT) | Reason (INFERENCE, from the run's own analysis) |
| --- | --- | --- |
| R001 MILBFA | **Pre-registered:** early stop. The low-attention segment had 12 to 23 next-season contributors per year league-wide, below the minimum of 30. No model was fitted, and the 2025 holdout is untouched. **Exploratory, no claim:** model M roughly ties B0, the "played in MLB last year" ranking (2024 AUROC difference −0.016, 95% −0.049 to 0.014) | The segment holds under one contributor per club per year (CALC: 23 / 30 MLB clubs = 0.77 in the highest year). In the exploratory whole pool, the obvious rule already found what the model found |
| R003 SENDHOLD | 2026 holdout, 1,927 sends, 71 outs at home. Brier score: model 0.0354, constant 0.0355, difference −0.0001, 95% [−0.0003, 0.0005]. The pre-registered gate failed, so NEGATIVE. The v2 definition and both sensitivity analyses were also NEGATIVE | Coaches already send almost only runners who score: about 96% success in 2026; the 2025 out rate was 3.6%. Season-level speed, arm, hit zone and outs do not separate the failures |

What the method establishes (FACT):
- **R001:** under the pre-registered design, the low-attention segment is too thin to test, as recounted from raw season files. The 2025 holdout is still available for one confirmatory test.
- **R003:** on the public Stats API data, a reduced logistic model did not beat a constant send-success rate on a holdout used once. The full covariate set was not fitted: it needs 140 outs and 2025 had 67.
- **R003 label amendment.** The primary label changed after retrieval and before any fit (v3, approved by the owner), because the feed splits one continuous advance at third base: about 94% of v2 "AMBIGUOUS" runners scored on the hit itself. The original v2 definition was also NEGATIVE. The label rule had been checked on a sample, not on the full retrieved data. Prompt v3 added a lesson against this.
- **R003 "runs left":** the estimate is 327 runs in 2026, and its interval excludes zero. The run judged it not interpretable: with a flat model it only restates the 96% send success. INFERENCE: the Brier gate worked as designed by blocking it.
- **R003 holds:** the counterfactual for a hold is unobservable (the feasibility run was PARTIAL, PR #6), and the Brier test scores only sends. So the result cannot show that coaches were right to hold. A 96% send success rate is also consistent with under-sending.

What the Cubs-specific evidence indicates (FACT, descriptive only):
- **MILBFA:** the Cubs signed 14, 12, 10 and 8 November-pool players in 2021 to 2024.
  - In the low-attention segment they had 1, 0, 1 and 0 of the league's 14, 12, 22 and 23 contributors. 2024 was the validation year.
  - R001 estimates that even perfect selection there would have added roughly one player a year. This is INFERENCE, a counterfactual that assumes those players were signable; the segment's league totals over 30 clubs give 0.4 to 0.8 per club per year.
  - This does not evaluate the Cubs' decisions. Availability, terms, competing offers and roster needs are unknown.
- **SENDHOLD:** 157 Cubs opportunities in 2025 and 159 in 2026. Their flagged holds carry the same caveat as the "runs left" estimate, so they establish nothing about the Cubs' coaching.

What remains speculative:
- **SENDHOLD (INFERENCE from R003):** the likely deciding information is play-level tracking. That means runner position and speed at the fielder's pickup, and throw distance and accuracy, none of which is in the Stats API feed. Whether a club with that data could gain an edge is SPECULATIVE.
- **MILBFA:** R001 does not identify a missing data source. Its limits are:
  - the outcome measures playing time, which reflects opportunity as well as talent;
  - there is no public value metric (WAR) in this source;
  - projection systems are not lawfully bulk-accessible for comparison;
  - samples per segment are thin;
  - the two methods for separating minor-league from MLB elections disagree on about 15% of cases.
- The ceiling of about one player per club per year applies to the low-attention segment only. The whole pool was examined only in the exploratory analysis.

**What was delivered** (cubs-edge-lab, public, main 97901d2):
- Retrieval tools for the public Stats API that respect the MLBAM terms. Bulk responses stay local; manifests and checksums are committed.
- **R001:** cohort construction with decision-time features, five baselines (B0, B1, B2, P and the model M) and rolling-origin evaluation, ending in a pre-registered early stop.
- **R003:** a frozen design (v2, amended to v3 before fitting) with one holdout evaluation, ending NEGATIVE.
- A static results explorer (R004, R005). It shows every published number exactly as computed, with each verdict and caveat beside the number it qualifies. Tests prove:
  - each displayed number equals its source value under the shared display format;
  - verdict-first order;
  - keyboard disclosure, the skip link and visible focus;
  - no word or number breaks in table cells;
  - no horizontal page scroll at 320, 375 and 1280 px;
  - no runtime network requests.
- Readability was judged from controller screenshots only. Readability defects escaped every R004 test, and one minor item is still open: tall prose rows at 375 px. No real user tested the explorer, so its value to a decision maker is unproven.
- Repeated practical use: unproven for every artifact. R001 states this; R003 does not address it.

## 3. Ideas considered, and next opportunities

- Discovery (OPPORTUNITIES.md):
  - R001: HowlDream divergent passes produced 45 and 49 idea units.
  - R003: pass 1 produced 53. A critique-informed pass 4 targeted high-frequency decisions (unit count not recorded).
  - Why SENDHOLD over MILBPROMO, which converged more strongly (5 of 6 against 4 of 6): MILBPROMO's promotion is selected on performance, so its counterfactual is unobserved. SENDHOLD's outcome and decision-time inputs were public, though in R003 its hold side also proved counterfactual.
  - Shortlists, rejections with reasons, and finalists are in OPPORTUNITIES.md.
- Not selected:
  - RULE5 was rejected after feasibility: explicit codes exist only from 2024.
  - CALLUP was rejected for R001 only: an in-season decision, with survivorship bias and strong public projections.
- Still open, from R003 (OPPORTUNITIES.md): COACH persistence, MiLB level-to-level promotion timing (converged in 5 of 6 pass-4 trials), and replay, starter-removal and intentional-walk decisions. None was feasibility-tested.
- In-game ideas (opposing bullpen, pitch-sequence predictability, tempo, disengagements) are parked. The Cubs' 2026 season ended on 2026-09-30 (OPPORTUNITIES.md, FACT from statsapi), so in-game tools could not be used before 2027.

Next experiments considered, with the evidence for and against each:

| Candidate | For | Against |
| --- | --- | --- |
| MILBFA confirmatory test on the untouched 2025 holdout | Public data; one pre-registered test is available (R001's stated strongest next step) | Needs a genuinely new feature hypothesis, and none has been identified. The low-attention ceiling is under one player per club per year. In the exploratory whole pool, the obvious rule already ties the model |
| New DISCOVERY_BUILD, or a feasibility-only FOLLOW_UP on an open finalist | The opportunity space is not exhausted. COACH is an offseason decision (October to December), so it is current now, and it uses cached stats | COACH has a few dozen movers per year, and those movers are selected (OPPORTUNITIES.md: "null likely", a prior, not a feasibility result). MILBPROMO's promotion is selected on performance, so its counterfactual is unobserved. REPLAY, STARTER and IBB rely on private decision-time video or are well studied, so parity is likely. Feasibility has not been assessed for any of them |
| R003 reanalysis by pooling seasons to reach 140 outs | No new data needed. It would show whether the full covariate set changes the verdict | The 2026 holdout has already been used once. Pooling it makes the result exploratory, not confirmatory (a judgment, not a falsification) |
| SENDHOLD with play-level tracking | Targets exactly the gap R003 identified | Needs non-public data, outside the campaign's data policy. That is an owner decision, not a campaign experiment |

INFERENCE: each of these either lacks a ready hypothesis, needs non-public data or the 2027 season, or has low expected value by prior. The cheapest real option is a feasibility-only COACH check. That supports a pause to ask (section 19), not a stop. Stopping is the owner's decision.

## 4. Howl assessment

### Components and contributions

| Component | Use | Contribution |
| --- | --- | --- |
| HowlDream | R001: 34 of 40 calls (discovery, critique, selection challenge, three prompt-review rounds). R003: 27 of 40 at design freeze (assessment); the journal's closeout total is 33, including a 3-call prompt-v3 review, with 3 calls not itemized. R004: 4 calls for the spec critique (journal), plus two prompt-v4 review rounds. Synthesis: 6 (two rounds). All remote Claude, none mocked | Discovery moved from analyst dashboards to offseason and high-frequency decisions. Design v2 followed the R001 selection challenge, adding stronger baselines, the low-attention primary test and the early-stop rule. INFERENCE: that shaped the early stop. Prompt and synthesis reviews found real errors. They also made wrong claims when evidence was missing from their input: round 1 here wrongly disputed the 18 lessons and the idea-unit counts |
| HowlPlane | Planning, implementation, verification, independent review, rework, acceptance. Sessions: R001 15, R002 2, R003 5, R004 6 plus 2 repair, R005 2, synthesis 1 | Product code and Howl repairs were written in its sessions. Exceptions done by the controller outside Howl: R004's web/data regeneration, R001's history rewrite, and records and merges |
| howl CLI | Front door for orchestrate and agents doctor | Its README example failed until DOG-035 was fixed |
| HowlCreate, HowlWriter, HowlFrame, HowlProof, HowlForge | Not used | No distinct role in this workload |

### Defects found and fixed (all merged with regression tests)

| ID | Defect | Fix | Proof |
| --- | --- | --- | --- |
| DOG-035 | `--verify` rejected commands with flags, including the README example | howlplane #164, howl #16 | Post-merge public proof file (R001) |
| DOG-036 | Fixed, undocumented 300 s verification timeout | #164 (`--verify-timeout`) | Used in later sessions, for example R002 (1000 s) and R005 (900 s) |
| DOG-037 | AGY read-only roles not enforced; a reviewer rewrote research files, and a verified implementation was lost | #165 | Live: AGY skipped for read-only roles (R001 S1 rerun) |
| DOG-038 | No supported worker network access for public data | #165 (`--worker-network`) | Live: Codex fetched public data (R001 S1 rerun) |
| DOG-039 | A Codex usage limit recorded as "not authenticated" | #166 | Regression tests and post-merge proof (R001) |
| DOG-040 | A refused test command marked a proven agent interactive-only everywhere | #167 | Reproduced once live (R002, session 55b942cf); session-scoped exclusions behaved as fixed in R003 |
| DOG-041 | Acceptance cited a superseded planner verify command | #167 | Live condition occurred without misattribution (R002); contract tests |
| DOG-042 | A declared INCOMPLETE was recorded as success | #167 | Contract tests (R002); live reroutes in R003 |
| DOG-043 | Correctable refusals reported as INTERNAL_ERROR | #167 | Live CLI on merged main (R002) |
| DOG-044 | Acceptance never saw HowlPlane's own verification result | #168 | Live: the same acceptor accepted, citing the harness (R004, fa728fa1) |

Review: each fix went through a HowlPlane session. The DOG-037/038 review ended COMPLETE WITH WARNINGS with audit CLEAN. The DOG-044 fix was made by session 03d1d55e, and its CI-only test was made hermetic by session d516f2a9. This synthesis also corrects the howlplane registry status lines for DOG-040 to 044, which still said FIX IN REVIEW. That was HowlPlane session 745c5055: review CLEAN on round 2, ACCEPTED, make test-full passed; howlplane PR #169, merged as 04b5269 with all CI checks green. Its DOG-042 line keeps the R002 scope ("not observed live"), as the goal specified; the R003 live reroute is noted in the PR.

### Open items (not repaired)

| ID | Note |
| --- | --- |
| CUBS-P-009 | No loopback-only network grant: browser tests need the full `--worker-network` |
| CUBS-P-011 | Claude's `--verify` grant is an exact literal and fails for long quoted gates. Mitigated with `sh scripts/gate.sh`. Claude is still refused ungranted targeted test commands, about one failover per session in R005. Granting `extra_allowed_bash` is the owner's configuration choice |
| CUBS-P-012 | A nonexistent `--repo` path is reported as INTERNAL_ERROR (found during this synthesis) |
| CUBS-P-013 | A CLEAN review whose status line is not last (Codex wrote it first, in bold) is treated as findings. It fails safe but cost one rework round, about 27 minutes, in the synthesis session 745c5055 |
| NOTE-005, 006, 007, 009, 010 | Dream budget validation; pooled assumptions in exports; a flaky hygiene test; the 1800 s worker cap; no re-judge after a harness-caused rejection |
| (unnumbered) | Acceptance applied criteria outside the goal, seen twice in R001 |

### Integration: strengths, failures and manual work

Failover rerouted work in every session in R003 to R005. It did not always finish the work:
- Sessions ended BLOCKED in R001 (S1, S2).
- Sessions ended HANDOFF in R004: S1, S2, and the CI-fix session d516f2a9 (journal 2026-10-10 23:35 UTC).
- R004-S2 was discarded.
- On resume, HowlPlane verified kept work directly, as designed.

Review and acceptance (mixed):
- **Caught:** stale reports, unsupported claims, an incomplete pipeline (R001), and `export_data` ignoring its root (R4-S5).
- **Escaped to the controller:**
  - per-segment double counting (R3-S1);
  - a .gitignore rule hiding web/data and a stale provenance commit (R004, found only by the fresh-clone check);
  - a CI-only test dependency (#168);
  - readability defects (R004 and R005, found by screenshot inspection).
- **False rejections:** acceptance rejected a passing tree three times (DOG-044) before the fix.
- **Controller error:** R3-S2 acceptance targets were taken from the defective artifact. The worker refused to match them, and a recount from raw data confirmed the worker.

Provider behavior:

| Agent | Behavior |
| --- | --- |
| Claude | Refused ungranted commands, including long gates (CUBS-P-011) |
| Codex | Usage limits (R001); the 1800 s cap |
| Cursor | Shell often rejected, even for no-ops |
| AGY | Mutated files in a read-only role (DOG-037), later skipped for read-only roles; usually made no repository change in R004 |
| Devin | Did not serve in R001; usually made no repository change in R004 |
| Vendor session limits | Handled by failover |

Local-LLM compliance: LOCAL_LLM_USED: NO in every run. HOWL_FORBID_LOCAL_INFERENCE=1 was set on every invocation, and all Dream and worker calls went to remote vendors.

### Limits on any claim of autonomous execution

This was not unattended autonomy. R001 alone had ten help requests, six of them formal PAUSED_NEEDS_HELP pauses (journal). The owner made or approved these decisions:
- repository visibility;
- repair authorizations: DOG-035, DOG-037/038, DOG-039 (then CUBS-P-007) and DOG-044;
- the data-terms decision and local-only Stats API use;
- the R003 feasibility sample (about 150 requests) and the two seasons (about 4,900 requests), both verbatim choices in the journal, and the v3 label amendment;
- two session-bound increases (R001);
- a history rewrite after bulk data was pushed (R001);
- acceptance decisions (R001).

Merges:
- R001 and R002: the controller merged under the owner's standing authorization for "routine mission-owned merges after review and checks" (CLOSEOUT-PROMPT.md).
- From R003 on, self-merge was blocked by the permission classifier. The owner merged, or I merged at the owner's explicit request.

The controller wrote goals, constraints and audits, and ran fresh-clone and screenshot checks. Several of those checks caught defects that Howl review did not.

### Controller deviations (disclosed in run assessments)

| Run | Deviation |
| --- | --- |
| R001 | Bulk MLB records pushed to a records branch, then removed by an authorized history rewrite. GitHub may still serve orphaned commits by SHA until a Support purge by the owner |
| R003 | Stacked PRs merged into their parents; recovered with #10 |
| R003 to R004 | R004 started (contract, spec critique, R4-S1 intent at 15:27Z) before R003 was MERGED_VERIFIED (#10 at 15:47Z). Section 19 says a run cannot advance until then. R4-S1 built on the branch #10 brought to main, and no work was lost. First disclosed in this synthesis (HowlDream round 2) |
| R004 | A session discarded without the owner's decision, against v3 section 19A |
| R004 | Product data files regenerated outside Howl, later superseded and re-verified |
| R004 | Two repair sessions ran beyond the 6-session bound |
| R004 | An R001 wording correction in research/, although the contract said unchanged; no value changed |

### Prompt evolution

- v2 added section 19A. v3 added four lessons and v4 seven. Section 19A now holds 18 lesson bullets (EXECUTION-PROMPT.md).
- Each version was reviewed by HowlDream before adoption. Hashes: v1 e1b5ca33…, v2 7fa1b0c1…, v3 2a958c2c…, v4 048f2f5b…
- R005 was the first run under v4. Its screenshot lesson caught the run's only escaped defect, and no further change was proposed.
- No prompt change is proposed from this synthesis. CUBS-P-012 was an operator typo, which a prompt rule would not prevent.

## 5. Decision (asked under PAUSED_NEEDS_HELP; answered: stop)

No experiment with a ready hypothesis and more than small expected value was identified. The owner's options:
1. **Stop the campaign** (STOPPED_BY_USER).
2. **Authorize a non-public or club-provided data source.** SENDHOLD with play-level tracking would then be the strongest candidate.
3. **Run a one-shot MILBFA confirmatory test on the 2025 holdout.** This needs a new feature hypothesis first, and the expected value is small.
4. **Run a feasibility-only FOLLOW_UP on COACH**, the one open offseason finalist, or a new DISCOVERY_BUILD that accepts an in-season experiment in 2027.
5. **Run Howl-only REGRESSION work** on CUBS-P-009, CUBS-P-011, CUBS-P-012, CUBS-P-013 and NOTE-010. This makes no baseball claim.

Recommendation (INFERENCE: further public-data work has low expected value): option 1. Option 4 (a COACH feasibility check) is worth choosing if one more bounded baseball run is wanted, and option 5 if more Howl dogfooding is wanted.

## 6. Durability and synthesis records

- Repository state at synthesis (REPOSITORIES.json and REPOSITORIES.md are refreshed with this synthesis, with branch, upstream, clean or dirty and push state):

  | Repository | State |
  | --- | --- |
  | cubs-edge-lab | Public, 97901d2 |
  | howlplane | 04b5269 (#169 merged: registry status; CI all green; registry on main checked) |
  | howl | 45478f4 |
  | howldream | f1cc5ca, unchanged |
  | howl-cubs-dogfood | Public; synthesis on records/synthesis |

- Reproduce the baseball results:
  1. Follow the cubs-edge-lab README to re-create data/ locally for individual use (MLBAM terms).
  2. Run `sh scripts/gate.sh`. On a fresh clone without data/, 6 README-documented tests skip.
  3. Open web/ for the explorer.
- Resume the campaign: read HOWL-CUBS-HANDOFF.md, then the last journal entries; reconcile with Git and `howl orchestrate inspect --json`; set HOWL_FORBID_LOCAL_INFERENCE=1; follow the owner's answer to section 5.
- Evidence: workflow-evidence/R001 to R005 and SYNTHESIS; runs/R00N/ (contracts, prompt snapshots, assessments); MISSION-JOURNAL.md (append-only); DOGFOOD.md.
- Synthesis delivery: see the delivery table in the handoff (HOWL-CUBS-HANDOFF.md) and the journal. The howlplane registry PR and the howl-cubs-dogfood records PR are tracked to MERGED_VERIFIED there.
- Synthesis review: HowlDream, remote claude-sonnet-5-5, 3 calls per round, LOCAL_LLM_USED: NO.
  - Round 1: hd-20261011-042812-3332507fffe7 (workflow-evidence/SYNTHESIS/dream/d1-synthesis-review-texts.txt).
  - Round 2: hd-20261011-043219-eaa237904514 (d2-synthesis-review-r2-texts.txt). Round 2 was given round 1's findings, an OPPORTUNITIES.md excerpt and section 19A.
- Round 1 reconciliation:
  - **Adopted:** the R001 framing (no model fitted; exploratory tie); "missing information" limited to R003; INFERENCE labels on the reasons; the per-run split of the robustness claims and disclosure of the R003 amendment; narrower explorer claims; failover and review overclaims removed; false rejections added; AGY's DOG-037 behavior; DOG-042's live evidence; the R003 Dream count; CALLUP and COACH status corrected; the run table; campaign state; the options list; help and limitations items.
  - **Not adopted,** because files the reviewer lacked support them: the idea-unit counts, the season end date and the 5-of-6 convergence (OPPORTUNITIES.md); the 18 lessons (EXECUTION-PROMPT.md). Round 2 confirmed these from the supplied excerpts.
  - **Record corrections:** the R004 repair-session count, the DOG-035 registry note, and DOG-039 in DOGFOOD.md.
- Round 2 reconciliation:
  - **Adopted:** the decision descriptions and why each mattered; "neither produced evidence"; R003 "as amended"; the Stats API scope of "not public"; the hold-side counterfactual; the 94% amendment reason and its label-check lesson; the INFERENCE label and per-club range for "perfect selection"; the R001 and R003 delivered-work split; R003 selection rationale against MILBPROMO; the survivorship claim corrected per finalist; COACH as the current offseason option; pause wording ("ready hypothesis and more than small expected value"); recommendation labelled INFERENCE; the R003 Dream count restated; review effectiveness softened; causal credit removed; the DOG-036 claim narrowed; failover scoped to R003 to R005; help count (ten requests, six pauses); merge authority cited; delivery SHAs added; the R003-to-R004 advance disclosed as a deviation.
  - **Not adopted, with the source the reviewer lacked:**
    - the CI-fix HANDOFF (journal 2026-10-10 23:35 UTC);
    - the R004 Dream count (journal 2026-10-10T15:27:02Z);
    - the merge authority (CLOSEOUT-PROMPT.md);
    - the R003 owner decisions (journal, verbatim choices);
    - the committed checksums (cubs-edge-lab README: SHA-256 in research/raw_manifest.json);
    - the v3 hash (EXECUTION-PROMPT.md v4 header);
    - the 30 MLB clubs (general fact, used only in a labelled CALC).
  - Records merges for R001 to R005: howl-cubs-dogfood PRs #1 to #6 are all merged (git log of main).
