# HOWL-CUBS HANDOFF (recovery entry point)

Updated: 2026-10-08T14:40Z

## Mission and state
- Mission: discover and test where software could create a meaningful baseball advantage for the Chicago Cubs, while dogfooding the Howl ecosystem (HowlDream discovery, HowlPlane lifecycle).
- Campaign status: ACTIVE. Operating mode: USER MODE. Phase: R001 discovery (Dream pass 1 running).
- Run: R001, type DISCOVERY_BUILD. Prompt: EXECUTION-PROMPT.md v1 sha256 e1b5ca33…a5439d6. Snapshot: runs/R001/PROMPT-SNAPSHOT.md. Contract: runs/R001/RUN-CONTRACT.md.
- Hypothesis: not yet selected. Opportunity: not yet selected. Project: not yet selected. Experiment: not yet selected.

## Versions
- howlplane main 4b7fe81 (engine src == bb7ba21). howldream main f1cc5ca. howl c9d37d5 (binary built 2026-10-07; version reports commit unknown). Full table: REPOSITORIES.json / REPOSITORIES.md.
- Baseball project SHA: none.

## Findings and repairs
- Active DOG finding: none. Owning repo: none. Repair branch/worktree: none. Next global ID believed DOG-035 (re-check registry before allocating).

## Git / publication
- Control repo: local `main`, no commits yet. Remote: to be created PUBLIC at github.com/howlcipher/howl-cubs-dogfood (user decision 2026-10-08). Baseball repo: PUBLIC under howlcipher, name after discovery.
- PR/check/merge state: none.

## Research and validation
- Data cutoff: not yet set. Holdout: not yet defined.

## Last successful action
- Dream pass-1 request validated (`howldream validate` -> valid).

## Next action
- Wait for Dream pass 1 (background) to finish; inspect `workflow-evidence/R001/dream/runs/hd-20261008-143010-528e5330b08f` (report.md, discovery.json, manifest.json); record exit status and LOCAL_LLM_USED evidence; then clustering/critique pass.
- Working directory: /run/media/system/tallgeese/dev/howl-cubs-dogfood
- Resume commands (verified):
  - `export HOWL_FORBID_LOCAL_INFERENCE=1`
  - `cat workflow-evidence/R001/dream/p1.exit` (absent = not finished or session died; check `ps -ef | grep 'howldream explore'` before relaunching)
  - `howldream inspect workflow-evidence/R001/dream/runs/<run-dir>`

## Active processes
- `howldream explore ... p1-divergent.request.json` launched 2026-10-08T14:30Z (background shell of this session).

## Bounds remaining (R001)
- Dream passes 5 (1 in use), remote calls 40 (up to 7 in use), pivots 1, HowlPlane sessions 8, repair attempts 2 per root cause.

## Warnings / blockers
- Shared engine: `dev/howlplane` checkout is used by other campaigns; re-verify HEAD and branch before every orchestrate.
- Other Claude/Codex sessions run on this host; do not touch their worktrees.
- Pending help request: none. Pending authority boundaries: none.

LOCAL_LLM_USED: NO (no provider invoked except remote Claude CLI via reviewed profile; to be confirmed from Dream manifest)
