# Dogfood finding index (howl-cubs mission)

Canonical DOG findings live in the HowlPlane registry: `howlplane/dogfood/findings/FINDINGS.md` on origin/main. This file indexes only findings raised or touched by this mission, with links to evidence. Provisional IDs use `CUBS-P-NNN` until reconciled.

Registry state at mission start (2026-10-08T14:27Z): highest allocated DOG-034; next DOG-035. Re-check origin/main before allocating.

## Findings

### DOG-035 (was CUBS-P-001): `--verify` cannot take a command containing a flag
- Category: HOWL FAILURE (broken documented CLI contract). Impact: high for ordinary users; the first-run README example fails. Owner: howlplane (parser), with docs in howlplane and howl.
- Expected: `howl orchestrate "Goal" --repo . --verify python3 -m unittest` (howl README.md:160) starts a session that verifies with that command.
- Observed: exit 2, `howlplane: error: unrecognized arguments: -m unittest`; no session created.
- Root cause: `--verify` uses argparse `nargs="+"` (src/howlplane/control_plane/orchestration.py:2003), which treats dash-prefixed command tokens as options. No CLI-level test covers it.
- Evidence: workflow-evidence/R001/plane/CUBS-P-001-repro.txt, S1.stderr.
- Status: FIX IN REVIEW. howlplane PR #164 (fe0ff37, CI green); howl PR #16 (docs). HowlPlane review session 1426222b: reviews CLEAN (Codex, Cursor), acceptance REJECTED for missing full gate (see CUBS-P-004); reviewed tree includes worker improvements (06-reviewed-tree.patch). Not merged.

### CUBS-P-002 (provisional): denial of an equivalent test command marks a proven agent interactive-only everywhere
- Category: HOWL FAILURE (false capability evidence, persisted). Owner: howlplane routing/readiness cache.
- Observed: existing-WIP validation; Claude ran `python3 -m pytest <same tests>` while the granted command was `pytest <same tests>`; denied with no repo change; readiness cache now says interactive-only (`howl agents doctor`).
- Expected: a refused test/verification command in a validation attempt should not erase previously verified unattended-edit capability for all repositories.
- Evidence: workflow-evidence/R001/repair-DOG-035/06-session-ledger.md attempt 2; agents doctor output in journal 17:45Z. Documented recovery exists: `howlplane agents doctor --live --agent claude_code --repo <repo>`.

### CUBS-P-003 (provisional): acceptance cites a superseded planner VERIFY_COMMAND as the session's verification command
- Category: HOWL FAILURE (review context). Owner: howlplane orchestration prompts.
- Observed: planner VERIFY_COMMAND named nonexistent tests/test_task_queue.py; explicit --verify superseded it; two acceptors reported "the supplied verification command names nonexistent tests/test_task_queue.py".
- Evidence: 06-session-ledger.md (planned_verify_command, attempts 6, 9, 13).

### CUBS-P-004 (provisional): fixed 300 s verification timeout makes some repositories' required gate unreachable
- Category: CAPABILITY GAP with defect aspects (undocumented limit). Owner: howlplane orchestration.
- Observed: VERIFY_TIMEOUT_SECONDS = 300 (orchestration.py:159), not configurable or documented; howlplane full suite ~456 s; acceptance requires the full gate; no session role can run it.
- Evidence: 06-session-ledger.md; 03-push-prepush-suite.log (2364 passed in 456.70 s).

## Capability notes and nonblocking friction

| ID | Component | Note | Evidence |
| --- | --- | --- | --- |
| NOTE-001 | howl / howlplane | `howl status` lists five installer-managed components and omits HowlDream (README: "HowlDream is not included in the default Howl installer"). Documented, not a defect. | workflow-evidence/R001/00-health/ |
| NOTE-002 | howlplane engine | Engine resolution prefers the Howl-managed `howlplane-engine` component over `HOWLPLANE_HOME`. Here the component is an editable install of the shared checkout, so the engine silently follows whatever branch any session checks out in `dev/howlplane`. Prior campaign records described this correctly in effect, but the path chain is non-obvious. | REPOSITORIES.json |
| NOTE-003 | howldream | Codex cannot be used as a clean Dream command provider without loading global ~/.codex/AGENTS.md ambient context. Claude can (`--setting-sources ""`). | MISSION-JOURNAL 14:33Z |
| NOTE-004 | howl | `howl version` reports commit "unknown" for a dev build, so executable-to-commit provenance for `howl` must be inferred from build time. | workflow-evidence/R001/00-health/ |
| NOTE-005 | howldream | `howldream validate` accepts an exploration request whose `budget.max_calls` cannot cover `baseline (clamp(max_candidates//2,1,3)) + max_candidates`; the run then ends PARTIAL/BUDGET_EXHAUSTED. Partial results were preserved correctly (documented). A validate-time warning would prevent it. Operator error + usability gap. | workflow-evidence/R001/dream/runs/hd-20261008-143010-528e5330b08f/manifest.json |
| NOTE-006 | howldream | `export --candidate-id <unit>` keeps only the IDEA sentence but attaches batch-level assumptions/unresolved items from the whole response. A downstream critic (Dream pass 2) flagged `CONFLICT: pooled_assumption_attribution` because tipping/grounds-crew assumptions were attached to the bullpen idea. Documented as an "explicit association limit"; still degrades critique input. Candidate for a unit-scoped assumption association. | workflow-evidence/R001/dream/p2-critique-texts.txt lines 48, 111, 165 |
