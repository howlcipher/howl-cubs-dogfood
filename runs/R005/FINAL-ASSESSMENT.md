# R005 final assessment

- Run: R005, FOLLOW_UP (explorer readability). Prompt: EXECUTION-PROMPT v4 (sha256 048f2f5b…), the first run under it.
- **Outcome: COMPLETE_USEFUL, bounded claim.** The three R004 readability defects are fixed, and screenshot inspection found and fixed a fourth (digits split across lines in data cells). Every R004 fidelity guard still passes. One minor item is open. This makes no claim of real user value: no user testing was available.
- **Delivery: PR_PENDING.**
  - cubs-edge-lab #16: one PR to main, not stacked. The repository has no CI; the PR says so.
  - howl-cubs-dogfood: records/R005.
  - howlplane, howl and howldream: NOT_APPLICABLE.

## Acceptance against the contract

| Criterion | Evidence | Result |
| --- | --- | --- |
| R004 guards keep their intent | Full suite on a fresh clone: 402 passed, 0 failed, 6 README-documented local-data skips | Met |
| No zero-reading text for a non-zero source; no "-0" | New tests on all four pages; Brier difference shows -0.000053 | Met |
| At most one full-value disclosure per row, covering every data-src in the row | New test | Met |
| No mid-word breaks; tables scroll in their container; no page scroll (320, 375, 1280 px) | Per-token single-line test on every th and td; screenshots | Met |
| Controller screenshots at 375 and 1280 px, from a fresh clone | workflow-evidence/R005/r5-*.png and r5b-*.png | Met. Found the data-cell defect, fixed in R5-S2 |
| Fresh-clone gate before PR; CI read before handoff | 0cc6bdc clone gate passed; CI absent, stated in #16 | Met |

Open (minor): at 375 px, the send/hold estimate column wraps its prose note into tall rows. It is readable, but long.

## Prompt v4 in practice

| v4 lesson | What happened in R005 |
| --- | --- |
| Fresh-clone gate (L1) | Ran at both commits, with skip reasons recorded |
| Gate command (L2) | `scripts/gate.sh` hash recorded before and after each session; unchanged |
| PR targeting (L3) | One PR to main, no stacking |
| CI before handoff (L4) | CI absent, stated in the PR |
| Screenshot inspection (L5) | Caught a defect the R5-S1 tests missed (they checked headers only). R5-S2 fixed it and added a per-token test |
| Protected inputs (L6) | No change to research/ or data/ (diff checked) |
| Repair sessions count (L7) | No repairs were needed |

## Howl observations

- HowlPlane sessions: 2 of 3 (7e3f20eb, ba91faa1). Both COMPLETE, review CLEAN, ACCEPTED, with no rework rounds.
- Failover in both sessions: Claude implemented or planned, was refused a targeted pytest command (not granted, extra_allowed_bash), and Codex completed. No HANDOFF, and no Cursor, AGY or Devin attempts were needed.
- No new Howl defects. LOCAL_LLM_USED: NO.

## Prompt retrospective

No change proposed. The v4 lessons fired as intended, and L5 found the run's only escaped defect. The recurring friction is Claude being refused ungranted test commands; it costs one failover per session. It is already recorded (CUBS-P-011 and the extra_allowed_bash note), and changing it is the owner's configuration choice, not a prompt matter.
