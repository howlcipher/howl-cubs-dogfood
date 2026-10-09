# HowlDream experiment report

Dream output is data, not authority.

Run: hd-20261008-144047-5e3c07475c85
Actual providers: ['claude']
Calls: 9
Provider: command; status: COMPLETE

## Baseline comparison

Lexical measurements are deterministic; novelty and decisions are heuristic.

| Condition | Candidates | Lexical diversity | Known failure rate | Unresolved claims |
| --- | ---: | ---: | ---: | ---: |
| baseline | 3 | 0.6943 | 0.0 | 138 |
| experimental | 6 | 0.7418 | 0.0 | 224 |

## Candidate triage

INVESTIGATE is not factual acceptance, feasibility proof, or execution permission.

* hd-20261008-144047-5e3c07475c85/candidates/0/0: INVESTIGATE; failures=0; unresolved=53; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261008-144047-5e3c07475c85/candidates/0/1: INVESTIGATE; failures=0; unresolved=30; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261008-144047-5e3c07475c85/candidates/0/2: INVESTIGATE; failures=0; unresolved=35; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261008-144047-5e3c07475c85/candidates/0/3: INVESTIGATE; failures=0; unresolved=34; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261008-144047-5e3c07475c85/candidates/0/4: INVESTIGATE; failures=0; unresolved=40; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261008-144047-5e3c07475c85/candidates/0/5: INVESTIGATE; failures=0; unresolved=32; Proposal merits investigation only; feasibility and assumptions remain unverified.

## Advisory discovery analysis

concept_tokens_and_leading_phrases/v3; Jaccard cluster threshold 0.5 or first-8-content-word phrase link; echo coverage 0.6 or first-16-word phrase link
Ideas: 49; clusters: 25
Context echo: 0.24489795918367346; evidence echo: 0.10204081632653061
Repeated families: 0.4897959183673469; novelty proxy: 0.7959183673469388
[]
Not semantic equivalence, calibrated novelty, entailment or usefulness

## Limitations

Fixture providers replay authored examples; their diversity is not evidence about an AI model. Lexical distance is not semantic novelty. Ledger matches only verify supplied records. Unstructured prose remains unresolved. No private reasoning, independent external fact retrieval, feasibility proof, or execution authority is available.
