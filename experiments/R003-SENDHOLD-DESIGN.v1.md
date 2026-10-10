# R003 experiment design: third-base send/hold (SENDHOLD)

Status: DRAFT v1, written 2026-10-10 before any full-season data was retrieved. The only outcomes seen so far are the 100-game feasibility sample (cubs-edge-lab research/SENDHOLD_FEASIBILITY.md). Changes after the independent challenge are recorded as v2 with reasons; this v1 stays as written.

## Decision and user

- User: a club's third-base coach and the advance/analytics staff who prepare send/hold guidance.
- Decision: with a runner on second at a single, or on first at a double, and fewer than two outs before the play, send the runner home or hold at third.
- Question: do major-league coaches hold runners whom public decision-time information says would score often enough to make a send worth it? If so, how many runs per season does that leave, league-wide and for the Cubs?

## Data (user authorization 2026-10-10, two seasons)

- Play-by-play for every 2025 and 2026 regular-season game (about 2,433 requests per season, 2 per second, cached under ignored data/), plus the public sprint-speed and outfielder arm-strength leaderboards. Bulk responses stay local; only aggregates and at most 5 short examples are published.
- Unit: one runner per qualifying play, with the base at contact taken from the runner's first movement segment (rule validated in R3-S2).
- Labels:
  - SEND: the runner's movement on the play ends in scoring or in an out at home.
  - HOLD: the runner ends at third and is not out.
  - Outs elsewhere, and other endings, are excluded and counted.
  - Runners who stop at third and then score later in the same play (the "advanced later" class in R3-S2) are ambiguous. They count as SEND in the primary analysis; a sensitivity analysis drops them.
- Covariates, all known at decision time or fixed per season:
  - Runner sprint speed (season leaderboard).
  - Arm strength of the fielder who fielded the ball (season leaderboard; missing for infielders, with a flag).
  - Hit type and hit location (coordinates and zone).
  - Outs, inning, and score difference.
- Season-level speed and arm values are known only after the season. This is a stated leakage limitation, and it is identical for sends and holds.

## Split and holdout

- Fit season: 2025. Holdout season: 2026, untouched until the model and thresholds are frozen and their hash is recorded in the journal.
- Run expectancy comes from the 2025 feeds only: average runs scored from each base-out state to the end of the inning, computed by the code. No outside run-expectancy numbers are used.

## Analysis

1. Send-success model: logistic regression of P(safe | SEND) on the covariates, fit on 2025 SEND runners. This is a deliberately simple, inspectable model.
2. Baseline for comparison: a constant (the 2025 SEND success rate), and an outs-only rate.
3. Break-even for each hold, from the 2025 run-expectancy table: p* = (RE[hold state] - RE[out-at-home state]) / (RE[run scored state] + 1 - RE[out-at-home state]). Trailing runners are ignored in this first version, as a stated simplification.
4. Flag a HOLD as a "missed send" when the predicted P(safe) is at least p* + 0.05.
5. Estimate the runs left per season as the sum, over flagged holds, of p·(RE_scored + 1) + (1 - p)·RE_out - RE_hold.
6. Overlap check: report the share of holds whose covariates lie inside the range covered by the sends (per covariate, within the 2.5th to 97.5th percentiles). Report results for the inside-range holds only.

## Pre-registered success and stop rules

- Early stop: if 2025 has fewer than 30 SEND runners thrown out at home, the success model is not fit. The result is reported as DESCRIPTIVE ONLY: rates, counts, and the observed out-at-home rate with its interval.
- Holdout validity: on 2026 SEND runners, the model must beat the constant baseline on Brier score (bootstrap over games, 95% interval of the difference excluding zero) and have a calibration slope between 0.7 and 1.3. If it fails, the result is NEGATIVE (the model cannot judge holds).
- Effect: a POSITIVE result requires the 2026 league-wide runs-left estimate to have a 95% game-bootstrap interval excluding zero. It is reported as an upper-bound-style estimate, because sends are selected and the counterfactual for holds is extrapolated.
- Cubs: count of Cubs flagged holds and their runs-left estimate in 2025 and 2026, descriptive only, with no significance claim.
- No re-tuning after the 2026 holdout is opened. Any later analysis is labeled EXPLORATORY.

## Known threats

- Selection: coaches send the runners they expect to be safe, so the model fit on sends is optimistic for holds. The overlap check and the upper-bound framing address this partly, not fully.
- Unobserved factors: the runner's jump, the outfielder's charge and exchange, the throw's accuracy, the coach's view of the play, and pitcher or batter context after the play.
- Missing defender arm values for infield-fielded balls.
- Trailing runners and later events are ignored in the run-value accounting.
- Season-level speed and arm values carry post-decision information.
