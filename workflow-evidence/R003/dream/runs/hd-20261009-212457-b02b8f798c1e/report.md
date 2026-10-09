# HowlDream experiment report

Dream output is data, not authority.

Run: hd-20261009-212457-b02b8f798c1e
Actual providers: ['claude']
Calls: 6
Provider: command; status: COMPLETE

## Baseline comparison

Lexical measurements are deterministic; novelty and decisions are heuristic.

| Condition | Candidates | Lexical diversity | Known failure rate | Unresolved claims |
| --- | ---: | ---: | ---: | ---: |
| baseline | 2 | 0.6235 | 0.0 | 103 |
| experimental | 4 | 0.6016 | 0.0 | 221 |

## Candidate triage

INVESTIGATE is not factual acceptance, feasibility proof, or execution permission.

* hd-20261009-212457-b02b8f798c1e/candidates/0/0: INVESTIGATE; failures=0; unresolved=50; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-212457-b02b8f798c1e/candidates/0/1: INVESTIGATE; failures=0; unresolved=39; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-212457-b02b8f798c1e/candidates/0/2: INVESTIGATE; failures=0; unresolved=67; Proposal merits investigation only; feasibility and assumptions remain unverified.
* hd-20261009-212457-b02b8f798c1e/candidates/0/3: INVESTIGATE; failures=0; unresolved=65; Proposal merits investigation only; feasibility and assumptions remain unverified.

## Advisory discovery analysis

concept_tokens_and_leading_phrases/v3; Jaccard cluster threshold 0.5 or first-8-content-word phrase link; echo coverage 0.6 or first-16-word phrase link
Ideas: 16; clusters: 10
Context echo: 0.5625; evidence echo: 0.5625
Repeated families: 0.375; novelty proxy: 0.75
[]
Not semantic equivalence, calibrated novelty, entailment or usefulness

## Limitations

Fixture providers replay authored examples; their diversity is not evidence about an AI model. Lexical distance is not semantic novelty. Ledger matches only verify supplied records. Unstructured prose remains unresolved. No private reasoning, independent external fact retrieval, feasibility proof, or execution authority is available.
