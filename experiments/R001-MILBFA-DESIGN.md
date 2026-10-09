# R001 experiment design: November free-agent pool triage (MILBFA)

Status: v2 (2026-10-09), revised after the early independent challenge (HowlDream pass 5, workflow-evidence/R001/dream/p5-challenge-texts.txt). v1 is preserved unchanged in R001-MILBFA-DESIGN.v1.md. No outcome label has been inspected for any cohort.

## Decision and user

- User: a club's pro scouting director and the front-office staff who decide minor-league contracts and non-roster invitations (NRIs).
- Recurring decision: every November, several hundred players elect free agency within days (S1: 585 to 908 "elected free agency" events per Nov-Dec window, 2018-2025). Staff must decide which few to pursue and in what order, with limited scouting attention.
- Intervention: a ranked list of the whole pool, built only from public information available on the decision date, with each player's evidence shown.

## Hypothesis (falsifiable)

H1: Ranking the November pool by a simple public-data model puts more next-season MLB contributors in the top k than a naive baseline ranking does.

- Population: every player with an "elected free agency" transaction (type DFA, description text "elected free agency") dated Nov 1 to Dec 31 of season Y. This avoids S1's unresolved minor-league vs MLB separation: the decision-maker sees the whole pool. A decision-time stratifier is reported separately: "no MLB appearance in season Y" (the low-attention segment), derived from season-Y stats.
- Outcome (binary, defined now): the player records at least 50 MLB plate appearances or at least 20 MLB innings pitched in season Y+1, for any club. Secondary: any MLB appearance in Y+1.
- Features: only data with event time and availability time no later than Dec 31 of season Y: age at Dec 31, highest level reached in Y, season-Y stats by level (rate stats and playing time), MLB playing time in Y and Y-1, position group (pitcher / hitter). No later transactions, no Y+1 data, no projections published after the decision date.

## Baselines (simple, comparable information)

- B0 (naive experience): rank by MLB playing time in season Y, then highest level reached, then younger first.
- B1 (simple rule a scout might use): rank by highest level reached in Y, then a level-appropriate rate stat (OPS for hitters, K-BB% for pitchers), then age.
- Model M: logistic regression on the features above, fitted on training cohorts only, preprocessing fitted on training data only. No hyperparameter search beyond a fixed, recorded regularization choice tuned on the validation cohort.

## Splits and holdout

- Training cohorts: 2018, 2019, 2021, 2022, 2023 (outcome seasons 2019, 2020, 2022, 2023, 2024). There is no 2020 cohort (no minor-league season in 2020). The 2019 cohort's outcome season is 2020 (shortened season): the outcome threshold is scaled by games played (60/162) and the cohort is also reported excluded as a sensitivity check.
- Validation cohort: 2024 (outcomes 2025), used once for the fixed regularization choice and for comparing M against B0/B1 during development.
- Final holdout: 2025 cohort (outcomes 2026, a season already complete on 2026-10-09). Not inspected for outcomes before the final evaluation. As of this writing the 2025 cohort's pool size is known from S1 (839 events), but no 2026 outcome has been retrieved or looked at.
- Players appear in multiple cohorts; the split is by cohort year (temporal), and the same player in different years is treated as separate decisions. Reported: how many holdout players also appear in training cohorts.

## Primary metric and uncertainty

- Primary: number of outcome-positive players in the top k = 50 of the ranking, holdout cohort, M vs B1 (the stronger simple baseline, chosen as whichever of B0/B1 is better on validation; recorded before holdout).
- Why k = 50: roughly the scale of minor-league contracts plus NRIs a club offers in an offseason (to be checked against public Cubs signings in the Cubs case; if the measured number differs a lot, k is re-justified before the holdout, not after).
- Secondary: precision@25, precision@100, AUROC, calibration of M.
- Uncertainty: paired bootstrap over players in the holdout cohort (2,000 resamples) for the difference in top-50 hits and AUROC; report 95% intervals.

## Practical significance and stopping

- Meaningful improvement: M finds at least 3 more outcome-positive players in its top 50 than the baseline, with the paired bootstrap interval excluding 0. Rationale: with base rates expected to be low, a few extra usable depth players per offseason is the scale at which a pro department's attention would shift; smaller differences are within scout-level noise.
- Rejection: if M does not beat the baseline by that margin on the holdout, the usefulness claim is rejected (COMPLETE_NEGATIVE), and no Cubs recommendation is made from M.
- Stop early (before modelling) if: outcome-positive base rate in training cohorts is so low (< 15 positives per cohort) that top-50 comparisons are not informative; record and use the pivot or stop.

## What this can and cannot establish

- Can: whether public decision-time data ranks the November pool better than simple rules, retrospectively.
- Cannot: causal benefit (outcome depends on opportunity given by the signing club), adoption, wins, or comparison with clubs' private evaluations.
- Known bias: a player's next-season MLB time partly reflects which club signed him and its roster needs.

## Cubs application (dated)

Retrospective case: as of 2025-12-31, apply the frozen method to the 2025 pool, list its top 50 and the "no MLB in 2025" segment, and compare with the minor-league contracts and NRIs the Cubs publicly signed that offseason (public transactions), then with 2026 outcomes. Report what the method would have flagged that the Cubs did not sign and vice versa, without claiming the method would have improved the Cubs' results. If H1 is rejected, the case illustrates limitations only.

## Smallest implementation

A CLI in cubs-edge-lab that builds the cohort tables from the existing probe cache plus the additional public queries it needs (season-Y stats and Y+1 outcomes per player), computes features with explicit as-of dates, fits M on training cohorts, evaluates M/B0/B1 with bootstrap intervals, and writes a dated ranked list for a chosen cohort. Bulk data stays local (MLBAM terms); committed outputs are aggregates and short examples.

## Bounds

HowlPlane sessions remaining for R001: 3 of 12 (user-raised bound). Data acquisition, build, and evaluation must fit; if not, pause and ask.

## v2 changes after the independent challenge (reconciled against evidence)

| Challenge | Decision | v2 rule |
| --- | --- | --- |
| B0 is a near-proxy of the outcome; B1 is a weak straw man | ACCEPTED | Add B2: logistic regression on 3 features (MLB PA+IP in Y, highest level in Y, age). Add persistence baseline P (Y+1 positive iff Y met the same threshold). Comparator = best of B0/B1/B2/P on validation, fixed before holdout. |
| Whole-pool AUROC can look strong from easy cases | ACCEPTED | PRIMARY evaluation is within the "no MLB appearance in Y" segment. Whole pool is secondary. Pitchers and hitters reported separately. |
| +3 threshold and k=50 unjustified; one holdout cohort is nearly a one-sample test | ACCEPTED | Success requires (a) holdout paired-bootstrap 95% lower bound of (M minus comparator) top-k hits > 0 in the primary segment, and (b) the same sign in at least 2 of 3 rolling-origin years (train on cohorts before t, test on t, t in {2021, 2022, 2023}). k swept over 25/50/100; primary k set from the measured number of minor-league contracts + NRIs the Cubs signed in recent offseasons, measured before the holdout. Effect sizes reported regardless. |
| Feature window through Dec 31 leaks post-election events | ACCEPTED | Features use season-Y game statistics (season ends before November) and transactions strictly before each player's election date. |
| Hindsight on the 2025 holdout | ACCEPTED (partly unavoidable) | Hash the frozen code, config and validation results and commit the hash before retrieving any 2026 outcome. The controller's general baseball knowledge of 2026 cannot be removed; recorded as a limitation. |
| Early stop at <15 positives is lax | ACCEPTED | Stop before modelling if the validation cohort's primary segment has fewer than 30 positives. |
| Outcome measures opportunity, not only value | ACCEPTED as limitation | Report "any MLB appearance in Y+1" as secondary; no public value metric (WAR) in this source; claims limited to "reached MLB playing time". |
| Compare with public projection systems | NOT TESTED (limitation) | No freely and lawfully bulk-accessible projections for minor-league free agents; recorded as UNKNOWN. |
| Name/ID matching risk | ACCEPTED | Join only on Stats API person IDs; no name matching. |
| A positive result alone is not a Cubs recommendation | ACCEPTED | Any Cubs output is labeled decision support only, with availability, cost and competing-bid information explicitly unknown. |
| Source terms vs cohort construction | OPEN (authority) | Pending user decision (journal 2026-10-09T07:00Z). |

## Pre-holdout decision: primary k (2026-10-09T10:05Z)

Measured (cubs-edge-lab research/data_summary.json, merged 0e53744; live recount of 2025-26 = 66 agreed): Cubs minor-league contracts per offseason 2021-22..2025-26 = 41, 38, 41, 65, 66; median `k_basis` = 41. Rule fixed now, before any outcome label exists: primary k = the smallest of {25, 50, 100} that is at least k_basis, so **primary k = 50**. k = 25 and 100 remain secondary.
