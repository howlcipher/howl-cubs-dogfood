# HowlDream experiment report

Dream output is data, not authority.

Run: hd-20261011-001943-4f5150f8a8c7
Actual providers: ['claude']
Calls: 3
Provider: command; status: COMPLETE

## Baseline comparison

Lexical measurements are deterministic; novelty and decisions are heuristic.

| Condition | Candidates | Lexical diversity | Known failure rate | Unresolved claims |
| --- | ---: | ---: | ---: | ---: |
| baseline | 1 | 0.0000 | 0.0 | 39 |
| experimental | 2 | 0.5007 | 0.0 | 87 |

## Candidate triage

INVESTIGATE is not factual acceptance, feasibility proof, or execution permission.

* hd-20261011-001943-4f5150f8a8c7/candidates/0/0: INVESTIGATE; failures=0; unresolved=34; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261011-001943-4f5150f8a8c7/candidates/0/1: INVESTIGATE; failures=0; unresolved=53; Proposal merits investigation only; feasibility and assumptions remain unverified.

## Advisory discovery analysis

concept_tokens_and_leading_phrases/v3; Jaccard cluster threshold 0.5 or first-8-content-word phrase link; echo coverage 0.6 or first-16-word phrase link
Ideas: 16; clusters: 13
Context echo: 1.0; evidence echo: 1.0
Repeated families: 0.1875; novelty proxy: 0.25
['DISCOVERY_ANCHORING_HIGH']
Not semantic equivalence, calibrated novelty, entailment or usefulness

## Limitations

Fixture providers replay authored examples; their diversity is not evidence about an AI model. Lexical distance is not semantic novelty. Ledger matches only verify supplied records. Unstructured prose remains unresolved. No private reasoning, independent external fact retrieval, feasibility proof, or execution authority is available.
