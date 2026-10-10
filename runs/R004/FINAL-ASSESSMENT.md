# R004 final assessment

- Run: R004, FOLLOW_UP (a results explorer for R001 and R003). Prompt: EXECUTION-PROMPT v3.
- **Outcome: COMPLETE_USEFUL, for the bounded claim only.** The explorer shows the published numbers exactly and states every verdict and caveat beside the numbers it qualifies. That is proven by tests, and on a fresh clone. It makes no claim of real user value, and readability defects are documented below.
- **Delivery:**
  - cubs-edge-lab: MERGED_VERIFIED. #11, then #15 (which carried #12, #13 and #14, after they merged into their stacked parents). A fresh GitHub clone of main 5fc04e9 passes sh scripts/gate.sh: 321 passed, 6 data-dependent skips.
  - howlplane: PR_PENDING. #168 has all CI green after the hermetic-test fix 2eba9dc.
  - howl-cubs-dogfood: records on records/R004.
  - howl and howldream: NOT_APPLICABLE.

## Acceptance against the contract

| Criterion | Evidence | Result |
| --- | --- | --- |
| Every displayed number equals its source value | The DOM scan tags each number with `data-src` and compares it to the source under the shared display format, on all four pages | Met |
| Verdicts and caveats beside their numbers; runs left only as "not interpretable" | Guard tests for verdict-then-meaning order (DOM and 375 px box); runs-left text; `disclosed_only` nodes hidden on load on every page | Met |
| Keyboard-only navigation | Keyboard disclosure, skip-link focus and visible-focus tests | Met |
| Layout at 375 and 1280 px | No horizontal page scroll at 320, 375 and 1280 px (tested) | Met as tested; readability is poor at 375 px (see below) |
| No runtime network requests | Network test: no request outside the local server | Met |
| Full suite passes, including a clean clone | Final branch 6cd4a15: 327 passed, 0 skipped in the working tree; 321 passed with 6 data-dependent skips in a fresh `git clone` | Met |
| Independent HowlPlane review and acceptance | Sessions 040f9990, fa728fa1, 98385d60, 82ec9cb8 and fbe71810: review CLEAN and acceptance ACCEPTED (8814b377 was discarded) | Met |

## What the screenshots show (controller inspection, workflow-evidence/R004/r4-final-*.png)

The fidelity guards hold:
- the verdict comes first, followed by its fixed plain meaning;
- Met and Not met appear in text;
- runs left is hidden behind "Failed-model value (not for decisions)";
- the free-agent status reads "was not tested (pre-registered early stop)".

Readability is weak, and no test catches it:
1. Every number has its own block-level "Full published value" disclosure. An interval therefore renders as "(95% interval: -0.00 [box] to 0.00 [box] )".
2. The Brier differences (about −0.00005) display as "-0.00" at two decimals, which hides the result. The full value is one tap away, but the visible text says nothing.
3. At 375 px the criteria table squeezes its columns instead of scrolling inside its container, so headers break mid-word.

INFERENCE: the spec asked for exact, traceable values, and the tests prove exactly that. Readability was left to a comprehension check, which was not possible without users. A next version needs:
- one disclosure per row;
- significant-figure display for small differences;
- a minimum table width with scrolling inside the container.

## Deviation from the contract

The contract said research/*.json would be reused unchanged. R4-S4 corrected one R001 wording error in the generator and regenerated two lines:
- research/cubs_case.json /method_status;
- the matching FACT line in research/EXPERIMENT.md.

The old text, "failed its pre-registered test", contradicted validation.json (early_stop true, validation null, "validation primary positives below 30"). No value changed. It is a separate commit (913b3f7) in PR #13 for the owner to review.

## Howl observations

- HowlPlane sessions: 6 of 6 product sessions (S1, S2, S2b, S3, S4, S5) and 1 repair session (03d1d55e). The DOG-044 fix was proven live: the same Claude acceptor that rejected a passing tree three times cited the harness gate and accepted (fa728fa1).
- Failover worked in every session. Implementation was nonetheless rarely first-try:

  | Agent | What happened | Sessions |
  | --- | --- | --- |
  | Claude | Refused ungranted commands: regeneration, and long quoted gates (CUBS-P-011) | S2b, S3, S4, S5 |
  | Codex | Hit the 1800 s cap | S3 and the repair session |
  | Cursor | Shell rejected, even for no-ops | S1, S3, S4 |
  | AGY and Devin | Usually NO_REPOSITORY_CHANGE | — |

  Two sessions ended HANDOFF. On resume, HowlPlane verified the kept work directly, as designed.
- New:
  - DOG-044: fix in review, #168.
  - NOTE-010: a harness-caused rejection cannot be re-judged on the same tree; the only way out is discard.
  - CUBS-P-011: the exact-literal --verify grant is brittle. A short repository gate script avoided it.
- Review effectiveness:
  - The R4-S5 review found a real defect (`export_data` ignored its root), fixed in one rework round.
  - Two defects escaped review and working-tree gates; only the controller's fresh-clone check found them: the .gitignore rule hiding web/data (R4-S1), and the provenance commit that made a combined commit stale (R4-S4, fixed in R4-S5).
  - The readability defects escaped every gate.
- LOCAL_LLM_USED: NO (HOWL_FORBID_LOCAL_INFERENCE=1 on every invocation).

## Prompt retrospective (v4 candidates, not yet adopted)

The R004 Dream budget (one pass) went to the spec critique, so these candidates are not Dream-reviewed. Adopting them is the first step of the next run.

1. Before any PR, run the gate in a real `git clone` of the commit. It caught two defects in R004 that the working tree hid.
2. Make the verification command a short repository script (`sh scripts/gate.sh`) rather than an inline `sh -c` chain.
3. For stacked PRs, write the merge order and the retarget-to-main step in each PR body.
4. Before acceptance of any UI work, the controller looks at screenshots at each tested viewport. Fidelity tests do not measure readability.
5. Record any change to inputs the contract calls "unchanged" as a contract amendment before the session that makes it.
6. Read the PR's CI checks before handing a PR to the owner. Local gates, and HowlPlane's verify on this machine, cannot catch tests that depend on installed agent CLIs (#168).
7. Stacked PRs merged into their parents twice (R003, R004). Prefer one PR per run to main, or make the merge order and retargeting a controller step before handing PRs over.
