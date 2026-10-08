# HowlDream experiment report

Dream output is data, not authority.

Run: hd-20261008-143010-528e5330b08f
Actual providers: ['claude']
Calls: 7
Provider: command; status: PARTIAL

## Baseline comparison

Lexical measurements are deterministic; novelty and decisions are heuristic.

| Condition | Candidates | Lexical diversity | Known failure rate | Unresolved claims |
| --- | ---: | ---: | ---: | ---: |
| baseline | 3 | 0.6849 | 0.0 | 195 |
| experimental | 4 | 0.7560 | 0.0 | 208 |

## Candidate triage

INVESTIGATE is not factual acceptance, feasibility proof, or execution permission.

* hd-20261008-143010-528e5330b08f/candidates/0/0: INVESTIGATE; failures=0; unresolved=70; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261008-143010-528e5330b08f/candidates/0/1: INVESTIGATE; failures=0; unresolved=38; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261008-143010-528e5330b08f/candidates/0/2: INVESTIGATE; failures=0; unresolved=46; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261008-143010-528e5330b08f/candidates/0/3: INVESTIGATE; failures=0; unresolved=54; Proposal merits investigation only; feasibility and assumptions remain unverified.

## Advisory discovery analysis

concept_tokens_and_leading_phrases/v3; Jaccard cluster threshold 0.5 or first-8-content-word phrase link; echo coverage 0.6 or first-16-word phrase link
Ideas: 45; clusters: 23
Context echo: 0.6666666666666666; evidence echo: 0.6666666666666666
Repeated families: 0.4888888888888889; novelty proxy: 0.7555555555555555
['DISCOVERY_ANCHORING_HIGH']
Not semantic equivalence, calibrated novelty, entailment or usefulness

## Limitations

Fixture providers replay authored examples; their diversity is not evidence about an AI model. Lexical distance is not semantic novelty. Ledger matches only verify supplied records. Unstructured prose remains unresolved. No private reasoning, independent external fact retrieval, feasibility proof, or execution authority is available.
