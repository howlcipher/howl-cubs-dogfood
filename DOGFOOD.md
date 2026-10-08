# Dogfood finding index (howl-cubs mission)

Canonical DOG findings live in the HowlPlane registry: `howlplane/dogfood/findings/FINDINGS.md` on origin/main. This file indexes only findings raised or touched by this mission, with links to evidence. Provisional IDs use `CUBS-P-NNN` until reconciled.

Registry state at mission start (2026-10-08T14:27Z): highest allocated DOG-034; next DOG-035. Re-check origin/main before allocating.

## Findings

None yet.

## Capability notes and nonblocking friction

| ID | Component | Note | Evidence |
| --- | --- | --- | --- |
| NOTE-001 | howl / howlplane | `howl status` lists five installer-managed components and omits HowlDream (README: "HowlDream is not included in the default Howl installer"). Documented, not a defect. | workflow-evidence/R001/00-health/ |
| NOTE-002 | howlplane engine | Engine resolution prefers the Howl-managed `howlplane-engine` component over `HOWLPLANE_HOME`. Here the component is an editable install of the shared checkout, so the engine silently follows whatever branch any session checks out in `dev/howlplane`. Prior campaign records described this correctly in effect, but the path chain is non-obvious. | REPOSITORIES.json |
| NOTE-003 | howldream | Codex cannot be used as a clean Dream command provider without loading global ~/.codex/AGENTS.md ambient context. Claude can (`--setting-sources ""`). | MISSION-JOURNAL 14:33Z |
| NOTE-004 | howl | `howl version` reports commit "unknown" for a dev build, so executable-to-commit provenance for `howl` must be inferred from build time. | workflow-evidence/R001/00-health/ |
