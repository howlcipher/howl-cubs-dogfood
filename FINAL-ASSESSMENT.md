# Campaign assessment index

| Run | Type | Outcome | Delivery | Prompt | Assessment |
| --- | --- | --- | --- | --- | --- |
| R001 | DISCOVERY_BUILD (MILBFA) | COMPLETE_NEGATIVE | MERGED_VERIFIED (cubs-edge-lab f7caca6; howlplane af9f40a; howl 45478f4; records merged at closeout) | v1 | [runs/R001/FINAL-ASSESSMENT.md](runs/R001/FINAL-ASSESSMENT.md) |
| R002 | REGRESSION (DOG-040..043) | COMPLETE_REGRESSION | MERGED_VERIFIED (howlplane 99d3aa4) | v2 | [runs/R002/FINAL-ASSESSMENT.md](runs/R002/FINAL-ASSESSMENT.md) |
| R003 | DISCOVERY_BUILD (SENDHOLD) | COMPLETE_NEGATIVE | MERGED_VERIFIED (cubs-edge-lab 91f06c5 via #6, #7, #10) | v2 | [runs/R003/FINAL-ASSESSMENT.md](runs/R003/FINAL-ASSESSMENT.md) |
| R004 | FOLLOW_UP (results explorer) | COMPLETE_USEFUL (bounded: fidelity of displayed results; readability defects documented) | MERGED_VERIFIED (cubs-edge-lab 5fc04e9 via #11, #15; howlplane 5253f3f via #168) | v3 | [runs/R004/FINAL-ASSESSMENT.md](runs/R004/FINAL-ASSESSMENT.md) |
| R005 | FOLLOW_UP (explorer readability) | COMPLETE_USEFUL (bounded: readability fixed without weakening fidelity guards) | MERGED_VERIFIED (cubs-edge-lab 97901d2 via #16) | v4 | [runs/R005/FINAL-ASSESSMENT.md](runs/R005/FINAL-ASSESSMENT.md) |

Campaign state: STOPPED_BY_USER (2026-10-11; the owner chose "Stop the campaign" from the options in [CAMPAIGN-SYNTHESIS.md](CAMPAIGN-SYNTHESIS.md) section 5). R001 to R005 are closed and MERGED_VERIFIED, and so are the synthesis changes (howlplane #169, howl-cubs-dogfood #7).

Summary so far: public-data software for November minor-league free agency does not show a meaningful advantage. The low-attention pool holds under one next-season contributor per club per year, and a model only ties the "played in MLB last year" rule. Five Howl defects were found and fixed (DOG-035 to DOG-039); three are open for R002.

R003: public-data software for third-base send/hold decisions does not show an advantage. Coaches already send almost only runners who score (about 96% success), and public speed, arm, hit-zone and outs data cannot tell the failures apart on a 2026 holdout. Howl ran five sessions with live failover and no new defects; one capability note (NOTE-009, the 30-minute worker cap).

R004: a static explorer shows the R001 and R003 results with exact, source-traced numbers and every verdict and caveat beside the values it qualifies, proven by tests and on a fresh clone. Readability is weak (fragmented intervals, two-decimal Brier differences, squeezed tables at 375 px) and no test catches it. Howl: DOG-044 was fixed and proven live; failover carried every session; NOTE-010 and CUBS-P-011 were recorded.

R005: the three R004 readability defects are fixed, and screenshot inspection found and fixed a fourth, without weakening any fidelity guard. One minor item is open (tall prose rows at 375 px). Howl: two sessions, each with one failover after Claude was refused a targeted test command; no new defects.

Synthesis: two public-data candidates (MILBFA, SENDHOLD) produced no evidence of a Cubs advantage. For SENDHOLD the likely deciding input, play-level tracking, is not in the Stats API feed. For MILBFA the opportunity was too small to test. Howl: ten defects fixed and merged (DOG-035 to DOG-044); CUBS-P-009, CUBS-P-011, CUBS-P-012, CUBS-P-013 and five notes remain open.
