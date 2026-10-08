# Mission Journal (append-only)

Times are UTC. Each entry: event, decision or checkpoint, evidence link.

## 2026-10-08T14:20Z — Mission start (fresh)
- Mission root absent; no prior howl-cubs campaign. Seed prompt: `../howl-cubs-claude-ready-prompt.md` sha256 e1b5ca33…a5439d6, copied verbatim to EXECUTION-PROMPT.md (v1) and runs/R001/PROMPT-SNAPSHOT.md (read-only). The file contains only the executable prompt (no audit wrapper).
- Active controllers checked: other Claude/Codex sessions exist on this host (pids 10940, 19204, 2690545, 39850, 674796, 1379815), none operating on this mission root. The prior HowlPlane household campaign (howlplane-dogfood-campaign worktree) reports CLEAN PAIR ON MAIN, no open finding. Its state is not touched.
- Prior art found: howl-creative-dogfood/run04_cubs_baseball (2026-10-02) built ArsenalWatch (repertoire change alerts), WrigleySentry (Wrigley microclimate positioning), challengeev (ABS challenge break-even). Those used Dream+Create with agents implementing directly, not HowlPlane. Supplied to Dream as `explored_families` (declared diversity treatment) so discovery seeks the next layer.

## 2026-10-08T14:25Z — Sync and provenance
- All participating repos fetched OK and current; howlframe fast-forwarded bd3c2a9 -> 8793d6e (clean, --ff-only). See REPOSITORIES.json.
- LIBRARY CONFLICT: prior campaign handoff names howlplane main bb7ba21; live checkout is 4b7fe81. Resolved: 4b7fe81 is the records-PR merge (#163); `git diff bb7ba21 4b7fe81 -- src pyproject.toml` is empty.
- Engine provenance verified read-only: howl -> howlplane (Go 0.1.0) -> enginepath step 2 managed component -> runtime venv whose control_plane is an editable install of howlplane/src. Managed component outranks HOWLPLANE_HOME; it happens to point at the checkout.
- Health: `howl doctor` HEALTHY (workflow-evidence/R001/00-health/howl-doctor.txt). `howl agents doctor`: Codex, Claude READY (unattended verified); Cursor, AGY, Devin READY (unattended unverified). All workers are remote vendor CLIs. Mission root not yet factory-prepared (not a target repo).
- DOG continuity: authoritative registry howlplane origin/main dogfood/findings/FINDINGS.md; highest allocated DOG-034; HANDOFF says next DOG-035. Re-check before allocation.

## 2026-10-08T14:28Z — Publication authority
- Asked the user for remote destination/visibility. User asked whether private costs money; answered (free; 2,000 Actions min/month, no enforced branch protection on Free). User chose: BOTH PUBLIC under github.com/howlcipher. Consequence: sanitize every push; no private orchestration state, transcripts with credentials, or restricted data.

## 2026-10-08T14:33Z — Dream provider decision
- Dream default allowlist is mock (NO_EFFECT); real discovery needs a reviewed command profile. Profiles: config/dream-claude-opus.json, config/dream-claude-sonnet.json (tools "", empty MCP, setting-sources "", hooks disabled, neutral system prompt, env_allowlist []).
- Codex rejected as a Dream command provider for now: `codex exec` would load the 12 KB global ~/.codex/AGENTS.md (ambient context Dream cannot measure); isolating it would require relocating credentials. Capability note, not a defect.

## 2026-10-08T14:35Z — INTENT: Dream pass 1 (divergent discovery)
- Command: `howldream explore workflow-evidence/R001/dream/p1-divergent.request.json --command-config config/dream-claude-opus.json --allow-remote --output workflow-evidence/R001/dream/runs` (HOWL_FORBID_LOCAL_INFERENCE=1). Budget max_calls 7.
