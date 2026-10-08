# Repositories (human view of REPOSITORIES.json)

| Repo | Path | Branch | Start SHA | Current | Modified by mission | Pushed | Notes |
| --- | --- | --- | --- | --- | --- | --- | --- |
| howl-cubs-dogfood (control) | dev/howl-cubs-dogfood | main | (none) | (none) | YES | NO, public remote to be created | Campaign records and prompt |
| baseball project | not yet selected | | | | | | Separate repo, public |
| howlplane | dev/howlplane | main | 4b7fe81 | 4b7fe81 | NO | n/a | Live engine (editable, via managed component) |
| howldream | dev/howldream | main | f1cc5ca | f1cc5ca | NO | n/a | `howldream` runs its editable venv |
| howl | dev/howl | main | c9d37d5 | c9d37d5 | NO | n/a | Go CLI; forwards orchestrate/agents/factory |
| howlcreate | dev/howlcreate | main | bab145a | bab145a | NO | n/a | |
| howl-provider-core | dev/howl-provider-core | main | d0054c4 | d0054c4 | NO | n/a | Dream transport |
| howlframe | dev/howlframe | main | bd3c2a9 | 8793d6e | NO | n/a | ff-only sync; PATH binary is release 0.1.0 |
| howlwriter, howlproof, howlforge, howlrelay | dev/… | main | see JSON | same | NO | n/a | Not yet used |

Execution chain for orchestrate: `howl` -> `howlplane` (Go 0.1.0) -> managed `howlplane-engine` wrapper -> runtime venv -> editable `dev/howlplane/src`.
