# R001 lessons (draft input for the section 18 prompt retrospective)

Each lesson: trigger evidence -> proposed instruction -> status.

1. Dream budget sizing. Trigger: pass 1 PARTIAL/BUDGET_EXHAUSTED (NOTE-005). Proposed: "Set Dream `max_calls` >= clamp(max_candidates//2,1,3) + max_candidates; record the computed number in the intent entry." Status: candidate.
2. Gate pushes on the secret scan. Trigger: journal 17:45Z checkpoint ran the scan and push in one command; the push happened before the (false-positive) matches were inspected. Proposed: "Run the secret/state scan as its own step; push only after its result is reviewed; scan for lease/fence tokens by value pattern, not by the words." Status: candidate.
3. Verification gate for Howl repairs. Trigger: review session 1426222b HANDOFF REQUIRED because the session's --verify was a narrow subset while acceptance applies the repository's TESTING.md full gate (DOG-036). Proposed: "When a HowlPlane session reviews a change to a repository with a required full gate, pass that gate as --verify with an adequate --verify-timeout." Status: candidate (depends on DOG-036 merge).
4. Engine provenance for candidate replays. Trigger: need to exercise an unmerged engine through the public chain without touching the shared checkout. Proposed: "For candidate public proof, run the committed candidate via PYTHONPATH=<clean worktree>/src and record SHA + help-text check; never switch the shared dev/howlplane checkout." Status: candidate.
5. Existing-WIP review side effects. Trigger: CUBS-P-002 (Claude marked interactive-only globally). Proposed: "After any HowlPlane session, check `howl agents doctor` for readiness downgrades and record/recover them through the documented live doctor." Status: candidate until CUBS-P-002 is repaired.
