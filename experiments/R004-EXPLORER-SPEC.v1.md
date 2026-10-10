# R004 explorer spec (v1)

Status: v1, 2026-10-10. Written before any build and pending an independent critique. Changes after the critique are recorded as v2, with reasons.

## Users and job

- Primary user: a front-office analyst or coach who does not read JSON or code.
- Job: see what each study found, how sure it is, and what it does **not** show, without being misled into using a failed model.

## Hard rules (misrepresentation guards)

1. **Exact values.** Every number shown is a value from cubs-edge-lab research/*.json. Display rounding is allowed only in the format stated next to the number, and the full value is available on hover or focus. No number is derived in the browser except those listed in this spec as computed, and each computed one is labeled "computed".
2. **Verdict first.** Each study page opens with its verdict (NEGATIVE, PARTIAL or EXPLORATORY, as published) and a one-sentence plain-language meaning, before any table.
3. **Failed model.** The send/hold model failed its pre-registered holdout. Model-based values (predicted P(safe), flagged holds, runs left) are never the default view, and each carries the label "Model failed holdout; not for decisions". The runs-left figure appears only inside the verdict criteria table, with the statement that it is not interpretable under the NEGATIVE verdict.
4. **Sample size.** Each chart cell or rate shows n. Cells the source marks low_n are visibly de-emphasized and say so in text, not only in color.
5. **Labels.** FACT, INFERENCE, UNKNOWN and EXPLORATORY labels from the source are kept verbatim.
6. **Attribution.** Every page carries the MLB Advanced Media attribution and the usage-restriction quote already in the README.

## Pages

1. **Overview**: the campaign question, a card per study (name, decision, verdict, one-sentence finding), and a "what this does not show" list.
2. **Send/hold (R003)**
   - Verdict panel: v3 primary verdict and the v2 pre-registered verdict, followed by the criteria table for each of the four analyses (v3 primary, v2, AMBIGUOUS as SENT_SAFE, fallback dropped).
   - Decision chart, from fit decision_chart cells:
     - Default columns: zone group, hit type, outs, speed tercile, n, n_sent, observed send success and break-even p*.
     - Filters: hit type, outs, zone, speed tercile.
     - Model mean P(safe) appears only behind a "show failed model values" toggle, with the label from rule 3.
   - Data: label counts per season under v3 and v2, the 2025 run-expectancy table with its counts, covariate coverage, and retrieval totals.
   - Feasibility: the sample verdict, its reason, and the count reconciliation.
   - Method notes: the design hash, the v1 to v3 history, and the fit and holdout hashes.
3. **Free-agent triage (R001)**: the validation results with early-stop status, the exploratory whole-pool results with an EXPLORATORY banner, and the Cubs case (signings, positives, segment) with its unknowns.
4. **About the data**: sources, terms, the request counts, and the statement "no live data; all values are from the published research files at commit X".

## Technical (ADR R004-1, option A)

- `python3 -m cubs_edge_lab.web_export` copies the needed research values into web/data/*.json, each with its source path and JSON pointer, and writes web/data/manifest.json with the cubs-edge-lab commit and SHA-256 of each source file. It never computes new statistics.
- web/: index.html plus one HTML page per study, with shared style.css and ES-module JavaScript. There are no external fonts, scripts or requests.
- Accessibility:
  - Semantic headings and tables (`<th scope>`, captions).
  - Labeled filter controls, keyboard operation, and visible focus.
  - WCAG AA contrast.
  - Verdict meaning carried by text, not color alone.
  - Light and dark color schemes.
- Responsive at 375 px (tables scroll inside their own container; the page itself never scrolls horizontally) and at 1280 px.
- Tests:
  - Export unit tests: exact copies, the pointer map, manifest hashes.
  - Fidelity tests: Playwright reads every rendered number tagged with `data-src="file#pointer"` and compares it with the source JSON under the stated rounding.
  - Guard tests: no model values are visible by default, the runs-left caveat is present, and the verdict precedes any table.
  - Keyboard test: the filters work without a mouse.
  - Viewport tests: no horizontal page scroll at 375 px.
  - Network test: no requests outside the local server.
  - End-to-end tests skip with a reason when Chromium is unavailable.

## Out of scope

Hosting or deployment, live data, new statistics, user accounts, and any write path.
