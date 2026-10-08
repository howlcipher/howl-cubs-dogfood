# Dogfood finding index (howl-cubs mission)

Canonical DOG findings live in the HowlPlane registry: `howlplane/dogfood/findings/FINDINGS.md` on origin/main. This file indexes only findings raised or touched by this mission, with links to evidence. Provisional IDs use `CUBS-P-NNN` until reconciled.

Registry state at mission start (2026-10-08T14:27Z): highest allocated DOG-034; next DOG-035. Re-check origin/main before allocating.

## Findings

### CUBS-P-001 (provisional; expected DOG-035): `--verify` cannot take a command containing a flag
- Category: HOWL FAILURE (broken documented CLI contract). Impact: high for ordinary users; the first-run README example fails. Owner: howlplane (parser), with docs in howlplane and howl.
- Expected: `howl orchestrate "Goal" --repo . --verify python3 -m unittest` (howl README.md:160) starts a session that verifies with that command.
- Observed: exit 2, `howlplane: error: unrecognized arguments: -m unittest`; no session created.
- Root cause: `--verify` uses argparse `nargs="+"` (src/howlplane/control_plane/orchestration.py:2003), which treats dash-prefixed command tokens as options. No CLI-level test covers it.
- Evidence: workflow-evidence/R001/plane/CUBS-P-001-repro.txt, S1.stderr.
- Status: OPEN, awaiting user response (campaign PAUSED_NEEDS_HELP). No repair or workaround applied.

## Capability notes and nonblocking friction

| ID | Component | Note | Evidence |
| --- | --- | --- | --- |
| NOTE-001 | howl / howlplane | `howl status` lists five installer-managed components and omits HowlDream (README: "HowlDream is not included in the default Howl installer"). Documented, not a defect. | workflow-evidence/R001/00-health/ |
| NOTE-002 | howlplane engine | Engine resolution prefers the Howl-managed `howlplane-engine` component over `HOWLPLANE_HOME`. Here the component is an editable install of the shared checkout, so the engine silently follows whatever branch any session checks out in `dev/howlplane`. Prior campaign records described this correctly in effect, but the path chain is non-obvious. | REPOSITORIES.json |
| NOTE-003 | howldream | Codex cannot be used as a clean Dream command provider without loading global ~/.codex/AGENTS.md ambient context. Claude can (`--setting-sources ""`). | MISSION-JOURNAL 14:33Z |
| NOTE-004 | howl | `howl version` reports commit "unknown" for a dev build, so executable-to-commit provenance for `howl` must be inferred from build time. | workflow-evidence/R001/00-health/ |
| NOTE-005 | howldream | `howldream validate` accepts an exploration request whose `budget.max_calls` cannot cover `baseline (clamp(max_candidates//2,1,3)) + max_candidates`; the run then ends PARTIAL/BUDGET_EXHAUSTED. Partial results were preserved correctly (documented). A validate-time warning would prevent it. Operator error + usability gap. | workflow-evidence/R001/dream/runs/hd-20261008-143010-528e5330b08f/manifest.json |
| NOTE-006 | howldream | `export --candidate-id <unit>` keeps only the IDEA sentence but attaches batch-level assumptions/unresolved items from the whole response. A downstream critic (Dream pass 2) flagged `CONFLICT: pooled_assumption_attribution` because tipping/grounds-crew assumptions were attached to the bullpen idea. Documented as an "explicit association limit"; still degrades critique input. Candidate for a unit-scoped assumption association. | workflow-evidence/R001/dream/p2-critique-texts.txt lines 48, 111, 165 |
