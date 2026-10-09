# HowlDream experiment report

Dream output is data, not authority.

Run: hd-20261009-190200-07312665f66f
Actual providers: ['claude']
Calls: 3
Provider: command; status: COMPLETE

## Baseline comparison

Lexical measurements are deterministic; novelty and decisions are heuristic.

| Condition | Candidates | Lexical diversity | Known failure rate | Unresolved claims |
| --- | ---: | ---: | ---: | ---: |
| baseline | 1 | 0.0000 | 0.0 | 19 |
| experimental | 2 | 0.5822 | 0.0 | 49 |

## Candidate triage

INVESTIGATE is not factual acceptance, feasibility proof, or execution permission.

* hd-20261009-190200-07312665f66f/candidates/0/0: INVESTIGATE; failures=0; unresolved=23; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-190200-07312665f66f/candidates/0/1: INVESTIGATE; failures=0; unresolved=26; Proposal merits investigation only; feasibility and assumptions remain unverified.

## Advisory discovery analysis

concept_tokens_and_leading_phrases/v3; Jaccard cluster threshold 0.5 or first-8-content-word phrase link; echo coverage 0.6 or first-16-word phrase link
Ideas: 11; clusters: 10
Context echo: 0.7272727272727273; evidence echo: 0.6363636363636364
Repeated families: 0.09090909090909091; novelty proxy: 0.8181818181818182
['DISCOVERY_ANCHORING_HIGH']
Not semantic equivalence, calibrated novelty, entailment or usefulness

## Limitations

Fixture providers replay authored examples; their diversity is not evidence about an AI model. Lexical distance is not semantic novelty. Ledger matches only verify supplied records. Unstructured prose remains unresolved. No private reasoning, independent external fact retrieval, feasibility proof, or execution authority is available.
