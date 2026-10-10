# Campaign assessment index

| Run | Type | Outcome | Delivery | Prompt | Assessment |
| --- | --- | --- | --- | --- | --- |
| R001 | DISCOVERY_BUILD (MILBFA) | COMPLETE_NEGATIVE | MERGED_VERIFIED (cubs-edge-lab f7caca6; howlplane af9f40a; howl 45478f4; records merged at closeout) | v1 | [runs/R001/FINAL-ASSESSMENT.md](runs/R001/FINAL-ASSESSMENT.md) |

| R002 | REGRESSION (DOG-040..043) | COMPLETE_REGRESSION | MERGED_VERIFIED (howlplane 99d3aa4) | v2 | [runs/R002/FINAL-ASSESSMENT.md](runs/R002/FINAL-ASSESSMENT.md) |
| R003 | DISCOVERY_BUILD (SENDHOLD) | COMPLETE_NEGATIVE | PR_OPEN (cubs-edge-lab #7-#9 stacked, awaiting user merge; #6 merged 537af76) | v2 | [runs/R003/FINAL-ASSESSMENT.md](runs/R003/FINAL-ASSESSMENT.md) |

Campaign state: ACTIVE. Next run: R004, chosen by evidence (see journal); prompt v3 takes effect there.

Summary so far: public-data software for November minor-league free agency does not show a meaningful advantage. The low-attention pool holds under one next-season contributor per club per year, and a model only ties the "played in MLB last year" rule. Five Howl defects were found and fixed (DOG-035 to DOG-039); three are open for R002.

R003: public-data software for third-base send/hold decisions does not show an advantage. Coaches already send almost only runners who score (about 96% success), and public speed, arm, hit-zone and outs data cannot tell the failures apart on a 2026 holdout. Howl ran five sessions with live failover and no new defects; one capability note (NOTE-009, the 30-minute worker cap).
