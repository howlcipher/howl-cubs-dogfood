# Prompt v4 reconciliation

The review used HowlDream in verification mode, on remote claude-sonnet-5-5, in two rounds of 3 calls each (p2 and p3, texts in dream/). This was a campaign-maintenance step after R004 closed, done on the owner's instruction "Do what you suggest" (to review and adopt the v4 candidates). It is not counted in R004's single Dream pass. LOCAL_LLM_USED: NO.

| Finding | Round | Decision |
| --- | --- | --- |
| L1 "full gate" undefined; clone skips can hide coverage | 1 | ADOPTED: same command as --verify; record pass, fail and skip counts and the reason for each skip; an undocumented skip is a finding that blocks the PR |
| L1 recovery gap; the controller could hand-fix the product | 1, 2 | ADOPTED: fixes go through Howl and count against the session bound. Restructuring unpushed commits is allowed only with an unchanged tree, then the gate is rerun |
| L1 drops the check-ignore candidate | 1, 2 | ADOPTED: covered by the gate's stray-ignored-paths check, otherwise `git check-ignore` |
| L1 clone source unclear; it shares this machine's tools | 1 | ADOPTED |
| L2 lets a worker write and narrow its own gate | 1 | ADOPTED: it runs the required gate unchanged; the review confirms it is not narrower; command and file hashes recorded before and after each session |
| L2 overclaims the CUBS-P-011 mitigation | 1, 2 | ADOPTED: "intended to avoid"; provisional; not known to prevent every provider's refusals |
| L2 burden: a new script in every repository | 1 | ADOPTED: an existing short command (make test-full) is acceptable |
| L3 one-PR rule poorly supported; 15:47:20Z trigger irrelevant | 1 | ADOPTED: the rule is now "target the default branch; stack only with a stated reason"; the trigger was replaced |
| L3 no recovery for merges into a non-default branch | 1 | ADOPTED: an identical-tree PR to the default branch; if the branch moved, gate the merge result |
| L3 retarget timing; merge authority | 2 | ADOPTED: retarget after the user merges the parent; the line grants no merge authority |
| L4 waiver on the controller's say-so; pending or absent CI; budget bypass | 1 | ADOPTED: no "ready" with a failing check; pending is handed off as pending after a recorded wait; fix sessions count |
| L4 outages and failures already on the default branch | 2 | ADOPTED: recorded with evidence and handed off as not verified by CI |
| L5 "accepting" ambiguous (authority); untestable | 1 | ADOPTED: in addition to Howl acceptance; controller-only (no local model); paths, viewports and findings recorded |
| L5 screenshots and third-party data | 1, 2 | ADOPTED: committed screenshots obey the third-party-data allowance |
| L6 self-authorized amendment | 1 | ADOPTED: a dated user decision under section 17 first |
| L6 keep the original text; mid-session discovery; detection | 1, 2 | ADOPTED |
| L6 overlap with the v3 label-freeze rule | 1, 2 | ADOPTED: "does not relax"; the overlap is intended (both require the user) |
| L7 new and unreviewed in round 1; "two repair sessions" unsupported | 2 | ADOPTED with correction: the claim is true (03d1d55e at 18:28 UTC, d516f2a9 at 22:30 UTC); the reviewer lacked the 18:28 entry, which is now cited |
| Header says seven, diff shows six | 2 | NOT ADOPTED: the final text has seven lessons; the reviewer saw a truncated diff |
| Mixed trigger time formats | 1, 2 | ADOPTED: triggers quote the journal heading as written (the journal changed format at 18:28 UTC; both forms come from `date -u`) |
| v4 sha256 not recorded | 1 | ADOPTED: recorded in the journal and in the R005 snapshot |

After round 2, all changes are wording tightenings that add no authority. No third round was run.
