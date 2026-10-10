# Campaign assessment index

| Run | Type | Outcome | Delivery | Prompt | Assessment |
| --- | --- | --- | --- | --- | --- |
| R001 | DISCOVERY_BUILD (MILBFA) | COMPLETE_NEGATIVE | MERGED_VERIFIED (cubs-edge-lab f7caca6; howlplane af9f40a; howl 45478f4; records merged at closeout) | v1 | [runs/R001/FINAL-ASSESSMENT.md](runs/R001/FINAL-ASSESSMENT.md) |

| R002 | REGRESSION (DOG-040..043) | COMPLETE_REGRESSION | MERGED_VERIFIED (howlplane 99d3aa4) | v2 | [runs/R002/FINAL-ASSESSMENT.md](runs/R002/FINAL-ASSESSMENT.md) |
| R003 | DISCOVERY_BUILD (SENDHOLD) | COMPLETE_NEGATIVE | MERGED_VERIFIED (cubs-edge-lab 91f06c5 via #6, #7, #10) | v2 | [runs/R003/FINAL-ASSESSMENT.md](runs/R003/FINAL-ASSESSMENT.md) |
| R004 | FOLLOW_UP (results explorer) | COMPLETE_USEFUL (bounded: fidelity of displayed results; readability defects documented) | MERGED_VERIFIED (cubs-edge-lab 5fc04e9 via #11, #15; howlplane 5253f3f via #168) | v3 | [runs/R004/FINAL-ASSESSMENT.md](runs/R004/FINAL-ASSESSMENT.md) |

Campaign state: ACTIVE. R004 closed (MERGED_VERIFIED). Next: review and adopt the prompt v4 candidates, an optional explorer readability follow-up, then the campaign synthesis closeout run.

Summary so far: public-data software for November minor-league free agency does not show a meaningful advantage. The low-attention pool holds under one next-season contributor per club per year, and a model only ties the "played in MLB last year" rule. Five Howl defects were found and fixed (DOG-035 to DOG-039); three are open for R002.

R003: public-data software for third-base send/hold decisions does not show an advantage. Coaches already send almost only runners who score (about 96% success), and public speed, arm, hit-zone and outs data cannot tell the failures apart on a 2026 holdout. Howl ran five sessions with live failover and no new defects; one capability note (NOTE-009, the 30-minute worker cap).

R004: a static explorer shows the R001 and R003 results with exact, source-traced numbers and every verdict and caveat beside the values it qualifies, proven by tests and on a fresh clone. Readability is weak (fragmented intervals, two-decimal Brier differences, squeezed tables at 375 px) and no test catches it. Howl: DOG-044 was fixed and proven live; failover carried every session; NOTE-010 and CUBS-P-011 were recorded.
