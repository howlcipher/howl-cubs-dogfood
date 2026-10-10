# R004 run contract

- Run ID: R004. Type: FOLLOW_UP (usability of the existing R001 and R003 results), not a new discovery.
- Prompt: EXECUTION-PROMPT v3 (sha256 2a958c2cf77ec82a75bec4b2ce3eb39c2310852db3ea2c0cfaa6b3859a65960b; snapshot PROMPT-SNAPSHOT.md).
- Question: can the published R001 (MILBFA) and R003 (SENDHOLD) results be made inspectable by a non-programmer (a front-office analyst or coach) without misrepresenting them, including their negative verdicts and caveats?
- Learning objectives:
  1. Howl: dogfood a workload unlike R001-R003. Frontend HTML/CSS/JavaScript, real-browser end-to-end tests, accessibility and responsive layout, and multi-file UI changes. R003 found no new Howl defects on the research workload, so this run tests whether Howl is as robust on a different one.
  2. Baseball usability: a bounded claim only, namely that the explorer shows the published numbers exactly and states every verdict and caveat. No new baseball-advantage claim; no claim about real user value, which would need user testing that is not available.
- Reused evidence: cubs-edge-lab research/*.json, as published on main, unchanged.
- New work: a static explorer in cubs-edge-lab web/, with a Python export step, unit tests, and Playwright end-to-end tests. See experiments/R004-EXPLORER-ADR.md.
- Acceptance:
  - Every displayed number is equal to its source JSON value, proven by a test.
  - Each verdict (NEGATIVE, PARTIAL, EXPLORATORY) and caveat appears next to the numbers it qualifies. In particular, "runs left" is shown only with the statement that it is not interpretable under the NEGATIVE verdict.
  - Keyboard-only navigation works.
  - Layout works at 375 px and 1280 px.
  - No network requests at runtime.
  - The full test suite passes, including in a clean clone.
  - Independent HowlPlane review and acceptance.
- Bounds:
  - HowlPlane sessions: 6.
  - Dream: 1 pass, at most 6 calls (a UX and misrepresentation critique of the spec).
  - Rework: 2 rounds per session, as the engine sets.
  - No new data retrieval, no spending, and no hosting or deployment. Publishing a site, such as GitHub Pages, would be a separate user decision.
- Standing authorizations: public repositories and routine mission merges. In practice, merges of my own PRs are blocked by the Claude Code permission classifier, so the user merges them.
