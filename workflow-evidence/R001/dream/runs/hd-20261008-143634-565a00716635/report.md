# HowlDream experiment report

Dream output is data, not authority.

Run: hd-20261008-143634-565a00716635
Actual providers: ['claude']
Calls: 6
Provider: command; status: COMPLETE

## Baseline comparison

Lexical measurements are deterministic; novelty and decisions are heuristic.

| Condition | Candidates | Lexical diversity | Known failure rate | Unresolved claims |
| --- | ---: | ---: | ---: | ---: |
| baseline | 2 | 0.5777 | 0.0 | 122 |
| experimental | 4 | 0.5966 | 0.0 | 210 |

## Candidate triage

INVESTIGATE is not factual acceptance, feasibility proof, or execution permission.

* hd-20261008-143634-565a00716635/candidates/0/0: INVESTIGATE; failures=0; unresolved=54; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261008-143634-565a00716635/candidates/0/1: INVESTIGATE; failures=0; unresolved=58; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261008-143634-565a00716635/candidates/0/2: INVESTIGATE; failures=0; unresolved=46; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261008-143634-565a00716635/candidates/0/3: INVESTIGATE; failures=0; unresolved=52; Proposal merits investigation only; feasibility and assumptions remain unverified.

## Advisory discovery analysis

concept_tokens_and_leading_phrases/v3; Jaccard cluster threshold 0.5 or first-8-content-word phrase link; echo coverage 0.6 or first-16-word phrase link
Ideas: 8; clusters: 6
Context echo: 0.125; evidence echo: 0.125
Repeated families: 0.25; novelty proxy: 1.0
[]
Not semantic equivalence, calibrated novelty, entailment or usefulness

## Limitations

Fixture providers replay authored examples; their diversity is not evidence about an AI model. Lexical distance is not semantic novelty. Ledger matches only verify supplied records. Unstructured prose remains unresolved. No private reasoning, independent external fact retrieval, feasibility proof, or execution authority is available.
