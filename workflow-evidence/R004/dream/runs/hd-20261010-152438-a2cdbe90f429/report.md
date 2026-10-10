# HowlDream experiment report

Dream output is data, not authority.

Run: hd-20261010-152438-a2cdbe90f429
Actual providers: ['claude']
Calls: 4
Provider: command; status: COMPLETE

## Baseline comparison

Lexical measurements are deterministic; novelty and decisions are heuristic.

| Condition | Candidates | Lexical diversity | Known failure rate | Unresolved claims |
| --- | ---: | ---: | ---: | ---: |
| baseline | 1 | 0.0000 | 0.0 | 32 |
| experimental | 3 | 0.5845 | 0.0 | 99 |

## Candidate triage

INVESTIGATE is not factual acceptance, feasibility proof, or execution permission.

* hd-20261010-152438-a2cdbe90f429/candidates/0/0: INVESTIGATE; failures=0; unresolved=47; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261010-152438-a2cdbe90f429/candidates/0/1: INVESTIGATE; failures=0; unresolved=27; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261010-152438-a2cdbe90f429/candidates/0/2: INVESTIGATE; failures=0; unresolved=25; Proposal merits investigation only; feasibility and assumptions remain unverified.

## Advisory discovery analysis

concept_tokens_and_leading_phrases/v3; Jaccard cluster threshold 0.5 or first-8-content-word phrase link; echo coverage 0.6 or first-16-word phrase link
Ideas: 35; clusters: 27
Context echo: 0.3142857142857143; evidence echo: 0.2857142857142857
Repeated families: 0.22857142857142856; novelty proxy: 0.8285714285714286
[]
Not semantic equivalence, calibrated novelty, entailment or usefulness

## Limitations

Fixture providers replay authored examples; their diversity is not evidence about an AI model. Lexical distance is not semantic novelty. Ledger matches only verify supplied records. Unstructured prose remains unresolved. No private reasoning, independent external fact retrieval, feasibility proof, or execution authority is available.
