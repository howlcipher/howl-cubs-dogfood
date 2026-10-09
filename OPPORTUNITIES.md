# Opportunities

Source of raw ideas: Dream pass 1 run `hd-20261008-143010-528e5330b08f` (45 IDEA units, `candidates.jsonl`; conventional baseline arm in `baseline.jsonl`). Unit IDs below are relative to that run (`candidates/<c>/<t>/idea/<n>`). Status values: SHORTLISTED, CRITIQUED, SELECTED, REJECTED, PARKED.

Decision context (FACT, statsapi, 2026-10-08): the Cubs' 2026 season ended in the Wild Card Series on 2026-09-30, so current decisions are offseason ones.

## Dream vs baseline (pass 1)

Both arms produced roster-rules bookkeeping, own-bullpen availability, MiLB promotion support, travel fatigue, arbitration comps, data-quality monitoring, weather contingency, and the same "unnecessary" and "clubs already ahead" zones. Units seen only in the Dream arm: opposing bullpen availability forecast (0/3/2), pitch-sequence predictability self-scout (0/2/3), pitch-clock tempo self-scout (0/2/4), opposing-manager tendencies (0/2/2, 0/3/3), disengagement-aware baserunning (0/0/12), cross-league rule-context normalization (0/2/6), rehab venue planner (0/3/5), non-ABS replay triage (0/2/1). Divergence over the baseline is real but modest.

## Shortlist (pass 2 critique input)

| Key | Unit | Problem / user / decision | Data (initial view) | Status |
| --- | --- | --- | --- | --- |
| ROSTER | 0/0/idea/2 | Roster-rules ledger; FO ops; options, DFA, 40-man, Rule 5 | Public transactions API; option-year history only partly public | SHORTLISTED |
| CALLUP | 0/0/idea/11 | MiLB call-up readiness calibration; analysts/FO; promote or not | Public MiLB stats (statsapi) | SHORTLISTED |
| OPPBULL | 0/3/idea/2 | Opposing bullpen availability forecast; manager/hitting coaches; lineup, platoon, pinch-hit timing | Public box scores / game logs | SHORTLISTED |
| PREDICT | 0/2/idea/3 | Self-scout pitch-sequence predictability; pitching coach/catchers; game-plan design | Public pitch-level data (Statcast via Savant) | SHORTLISTED |
| RTP | 0/1/idea/6 | Return-to-play comparables; medical/FO; IL timeline and contingency | Public IL transactions; injury descriptions coarse | SHORTLISTED |
| XLEAGUE | 0/2/idea/6 | Cross-league rule-context normalization; MiLB evaluators; promotion and comparison | Public MiLB stats + league rule calendars | SHORTLISTED |

## Not shortlisted at this stage (with reason)

| Unit(s) | Idea | Reason |
| --- | --- | --- |
| 0/0/1, 0/1/1, 0/3/9 | Day-game / travel / heat rest planners | Outcome (injury, fatigue) not observable in public data; Dream itself flagged heat planning as spreadsheet-sufficient |
| 0/0/5 | Own bullpen readiness ledger | Needs private warm-up logs; own staff already know availability |
| 0/0/10, 0/2/5, 0/3/6, 0/1/2 | Decision journal, institutional memory | Value depends on private club notes; not testable with public data |
| 0/0/8, 0/1/5 | Translation / multilingual plans | Not a data problem; existing tools and human translators |
| 0/1/7, 0/3/4 | Grounds-crew logging | Private inputs; tiny effect hard to measure |
| 0/0/13, 0/0/14 | IFA pool allocator, arbitration comps | Small N, sparse public outcomes; arbitration figures partly public |
| 0/0/3 | Waiver/DFA monitor | Plausible; parked as overlapping ROSTER/talent evaluation |
| 0/0/4, 0/3/1 | Own-pitcher tipping audit | Needs high-frame video; public data insufficient |
| 0/0/9, 0/2/2, 0/3/3, 0/0/12, 0/2/4, 0/2/1 | In-game cards, manager tendencies, disengagements, tempo, replay triage | PARKED: plausible, but in-game tools cannot be used until 2027; may return in the second divergent pass |
| 0/0/15, 0/1/8, 0/1/9, 0/2/9, 0/3/8 | Leaderboards, depth charts, lineup order, replay | Dream's own "unnecessary" verdicts; accepted |
| 0/0/16, 0/1/10, 0/2/10 | Pitch design / biomechanics / bat tracking | Dream's "clubs already ahead" verdicts; accepted |

## Dream pass 4 (second divergent, blind-spot informed)

Run `hd-20261008-144047-5e3c07475c85`: 49 IDEA units, 25 clusters, novel_concept_ratio 0.80, all remote claude-opus-5-5. New territory relative to pass 1: draft coverage and in-draft pool math, non-tender supply shock and own tender bias, trade throw-ins and Rule 5, FA aging curves by skill profile, MiLB defensive standards, remote S&C coordination, minor-league free agents, spring-stat bias, NPB/KBO/indy returnees, coaching-hire evaluation, position conversions, opt-out veterans, revealed preferences of other clubs, adverse selection in acquisitions, postseason roster shape, ABS catcher repricing, position-player pitching. Explicit "unnecessary" zones: relationship-driven and one-off decisions, rare decisions with near-zero spreadsheet error.

Convergence signal: minor-league free agency (six-year MiLB FA / November depth signings) appeared independently in 4 of 6 Dream trials (0/0/9, 0/1/1, 0/2/1, 0/3/6). Convergence is not evidence of value, only of salience under this prompt.

## Finalists for data-feasibility research (pre-selection)

| Key | Source units | Decision owner / recurring decision | Mechanism claimed | Main risks (from critique and controller audit) |
| --- | --- | --- | --- | --- |
| MILBFA | p4 0/0/9, 0/1/1, 0/2/1, 0/3/6 | Pro scouting director / which minor-league free agents get offers and NRIs each November | Coverage of a large low-attention pool; speed | Very low base rate of MLB value; outcome depends on opportunity given by the signing club; whether historical MiLB FA lists are reconstructible from public transactions is UNKNOWN |
| RULE5 | p4 0/3/5; p1 0/0/2 | Farm director + GM / November 40-man protection list and December Rule 5 picks | League-wide model of which unprotected players get taken and stick | ~10-20 selections per year (small N); eligibility needs signing dates/ages |
| CALLUP+XLEAGUE | p1 0/0/11, 0/2/6 | Analysts/FO / promotion and prospect comparison | Translation with walk-forward calibration | Survivorship; strong public projection systems; in-season decision |
| OPPBULL | p1 0/3/2 | Manager/bench / lineup and pinch-hit timing | Better forecast than naive rest rule | In-season only (not usable until 2027); may reproduce the naive rule |
| SPRING | p4 0/1/2 | GM/manager / final roster spots each spring | Commitment device against spring-stat bias | Effect of spring stats is widely studied publicly; a spreadsheet may suffice |

Selection is deferred until a Howl research workflow measures public data availability for the top finalists (MILBFA, RULE5, CALLUP+XLEAGUE).

## Selection (2026-10-09, controller decision on S1 evidence)

Evidence: cubs-edge-lab research/FEASIBILITY.md (S1c/S1d, accepted, merged 35f803b).

| Key | S1 verdict | Decision | Reason |
| --- | --- | --- | --- |
| MILBFA | PARTIAL | SELECTED | Every offseason pool is measurable (585-908 "elected free agency" events per Nov-Dec window, 2018-2025); outcomes (next-season MLB appearances) are public and arrive after the decision, so a temporal backtest is possible; the decision recurs every November with a named owner (pro scouting director). The PARTIAL part (minor-league vs MLB separation; methods disagree ~15%) is handled by design: the experiment does not depend on that separation (see design). |
| RULE5 | PARTIAL | REJECTED | Explicit R5 codes only from 2024 (15 MLB-phase picks in Dec 2024): too few historical labels for any evaluation. |
| CALLUP | PARTIAL | REJECTED (this run) | In-season decision (not usable before 2027), survivorship bias, strong public projection alternatives; S1 sample thin. |
| OPPBULL, SPRING | not probed | PARKED | In-season or widely studied; no feasibility evidence gathered. |

Pivot budget: unused (1 remaining).
