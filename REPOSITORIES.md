# Repositories (human view of REPOSITORIES.json)

Refreshed at the campaign synthesis (2026-10-11). All listed checkouts were clean on main at refresh, except the control repository on records/synthesis.

| Repo | Path | Branch | Start SHA | Current | Modified by mission | Pushed | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| howl-cubs-dogfood (control) | dev/howl-cubs-dogfood | records/synthesis -> main | 5d9235d | main 64e7969 (#1-#6 merged) | YES | YES, public | Campaign records and prompt |
| cubs-edge-lab | dev/cubs-edge-lab | main | 7c715c8 | 97901d2 | YES | YES, public (PRs #1-#16) | Bulk data local only; no CI; clone gate 402 passed |
| howlplane | dev/howlplane | main | 4b7fe81 | 5253f3f | YES | YES (#164-#168 merged; registry-status PR pending) | Live engine (editable, via managed component) |
| howldream | dev/howldream | main | f1cc5ca | f1cc5ca | NO | n/a | `howldream` runs its editable venv |
| howl | dev/howl | main | c9d37d5 | 45478f4 | YES | YES (#16 merged) | Go CLI; forwards orchestrate/agents/factory |
| howlcreate | dev/howlcreate | main | bab145a | bab145a | NO | n/a | |
| howl-provider-core | dev/howl-provider-core | main | d0054c4 | d0054c4 | NO | n/a | Dream transport |
| howlframe | dev/howlframe | main | bd3c2a9 | 8793d6e | NO | n/a | ff-only sync; PATH binary is release 0.1.0 |
| howlwriter, howlproof, howlforge, howlrelay | dev/… | main | see JSON | same | NO | n/a | Not yet used |

Execution chain for orchestrate: `howl` -> `howlplane` (Go 0.1.0) -> managed `howlplane-engine` wrapper -> runtime venv -> editable `dev/howlplane/src`.
