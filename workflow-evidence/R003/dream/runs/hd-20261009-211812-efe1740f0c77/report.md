# HowlDream experiment report

Dream output is data, not authority.

Run: hd-20261009-211812-efe1740f0c77
Actual providers: ['claude']
Calls: 9
Provider: command; status: COMPLETE

## Baseline comparison

Lexical measurements are deterministic; novelty and decisions are heuristic.

| Condition | Candidates | Lexical diversity | Known failure rate | Unresolved claims |
| --- | ---: | ---: | ---: | ---: |
| baseline | 3 | 0.6320 | 0.0 | 130 |
| experimental | 6 | 0.7612 | 0.0 | 198 |

## Candidate triage

INVESTIGATE is not factual acceptance, feasibility proof, or execution permission.

* hd-20261009-211812-efe1740f0c77/candidates/0/0: INVESTIGATE; failures=0; unresolved=48; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-211812-efe1740f0c77/candidates/0/1: INVESTIGATE; failures=0; unresolved=36; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-211812-efe1740f0c77/candidates/0/2: INVESTIGATE; failures=0; unresolved=27; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-211812-efe1740f0c77/candidates/0/3: INVESTIGATE; failures=0; unresolved=32; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-211812-efe1740f0c77/candidates/0/4: INVESTIGATE; failures=0; unresolved=31; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-211812-efe1740f0c77/candidates/0/5: INVESTIGATE; failures=0; unresolved=24; Proposal merits investigation only; feasibility and assumptions remain unverified.

## Advisory discovery analysis

concept_tokens_and_leading_phrases/v3; Jaccard cluster threshold 0.5 or first-8-content-word phrase link; echo coverage 0.6 or first-16-word phrase link
Ideas: 53; clusters: 30
Context echo: 0.2830188679245283; evidence echo: 0.2641509433962264
Repeated families: 0.4339622641509434; novelty proxy: 0.7924528301886793
[]
Not semantic equivalence, calibrated novelty, entailment or usefulness

## Limitations

Fixture providers replay authored examples; their diversity is not evidence about an AI model. Lexical distance is not semantic novelty. Ledger matches only verify supplied records. Unstructured prose remains unresolved. No private reasoning, independent external fact retrieval, feasibility proof, or execution authority is available.
