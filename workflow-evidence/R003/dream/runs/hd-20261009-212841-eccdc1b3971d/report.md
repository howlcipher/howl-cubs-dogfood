# HowlDream experiment report

Dream output is data, not authority.

Run: hd-20261009-212841-eccdc1b3971d
Actual providers: ['claude']
Calls: 9
Provider: command; status: COMPLETE

## Baseline comparison

Lexical measurements are deterministic; novelty and decisions are heuristic.

| Condition | Candidates | Lexical diversity | Known failure rate | Unresolved claims |
| --- | ---: | ---: | ---: | ---: |
| baseline | 3 | 0.5914 | 0.0 | 159 |
| experimental | 6 | 0.6856 | 0.0 | 249 |

## Candidate triage

INVESTIGATE is not factual acceptance, feasibility proof, or execution permission.

* hd-20261009-212841-eccdc1b3971d/candidates/0/0: INVESTIGATE; failures=0; unresolved=51; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-212841-eccdc1b3971d/candidates/0/1: INVESTIGATE; failures=0; unresolved=44; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-212841-eccdc1b3971d/candidates/0/2: INVESTIGATE; failures=0; unresolved=38; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-212841-eccdc1b3971d/candidates/0/3: INVESTIGATE; failures=0; unresolved=42; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-212841-eccdc1b3971d/candidates/0/4: INVESTIGATE; failures=0; unresolved=38; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-212841-eccdc1b3971d/candidates/0/5: INVESTIGATE; failures=0; unresolved=36; Proposal merits investigation only; feasibility and assumptions remain unverified.

## Advisory discovery analysis

concept_tokens_and_leading_phrases/v3; Jaccard cluster threshold 0.5 or first-8-content-word phrase link; echo coverage 0.6 or first-16-word phrase link
Ideas: 44; clusters: 20
Context echo: 0.2727272727272727; evidence echo: 0.1590909090909091
Repeated families: 0.5454545454545454; novelty proxy: 0.5
[]
Not semantic equivalence, calibrated novelty, entailment or usefulness

## Limitations

Fixture providers replay authored examples; their diversity is not evidence about an AI model. Lexical distance is not semantic novelty. Ledger matches only verify supplied records. Unstructured prose remains unresolved. No private reasoning, independent external fact retrieval, feasibility proof, or execution authority is available.
