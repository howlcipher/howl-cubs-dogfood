# R005 run contract (draft; starts when prompt v4 is merged)

- Run ID: R005. Type: FOLLOW_UP (readability of the R004 explorer). Prompt: EXECUTION-PROMPT v4 (sha256 048f2f5bcd32259298d625139e326da54ca45da9aed58d96719b395e3dff63b0), snapshotted at run start.
- Question: can the explorer be made readable on a phone and a desktop without weakening any R004 fidelity guard?
- Defects to fix (R004 assessment, controller screenshots workflow-evidence/R004/r4-final-*.png):
  1. Every number has its own block-level "Full published value" disclosure, which fragments intervals and rows.
  2. Small values round to "-0.00" under the two-decimal format and hide the result (Brier differences of about -0.00005).
  3. At 375 px the criteria table squeezes its columns, and headers break mid-word instead of the table scrolling inside its container.
- Acceptance:
  - Every R004 guard test still passes unchanged in intent: exact source values (DOM scan), verdict and meaning first, runs left hidden by default, Met/Not met in text, limits blocks, keyboard, the accessibility tests, no runtime network.
  - No rendered value rounds to zero when its source value is not zero, and no "-0" text appears (tested).
  - Each table row has at most one full-value disclosure, and every number in the row is listed in it with its source pointer (tested).
  - At 320, 375 and 1280 px: no horizontal page scroll; tables scroll inside their own container; no table header wraps mid-word (tested).
  - The controller inspects screenshots of all four pages at 375 and 1280 px from a fresh clone, and records the findings (prompt v4).
  - The gate passes on a fresh clone of the exact commit, and the PR's CI checks are read before handoff (prompt v4).
- Unchanged inputs: research/ and data/ are not edited. web/data may be regenerated only by the accepted exporter inside a HowlPlane session.
- Bounds:
  - HowlPlane sessions: 3, including any repair sessions (prompt v4 lesson 7).
  - Dream: none.
  - No new data retrieval, no spending, no hosting.
- One PR to cubs-edge-lab main. Verification command: `sh scripts/gate.sh` (content hash recorded before and after each session).
