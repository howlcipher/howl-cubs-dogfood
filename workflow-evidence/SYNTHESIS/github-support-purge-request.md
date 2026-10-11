# GitHub Support request: purge orphaned commits (ready to submit)

Submit at https://support.github.com/contact while signed in as the repository owner (howlcipher). Suggested category: "Removing sensitive data". The text below the line is the request.

Prepared 2026-10-11. Every SHA listed was checked that day with `gh api repos/howlcipher/howl-cubs-dogfood/commits/<sha>`, and every one returned HTTP 200.

---

**Subject:** Remove orphaned commits containing third-party data from howlcipher/howl-cubs-dogfood

Hello,

I rewrote the history of the branch `records/R001` in my public repository https://github.com/howlcipher/howl-cubs-dogfood on 2026-10-09 to remove files that contain bulk third-party data (MLB Stats API responses, whose terms permit only individual, non-bulk use). The rewritten branch was force-pushed (old tip 4e3fbaf03a69da667cfde63fc49f21a4f4a94b3b, new tip d7fa0f9) and later merged. The removed files no longer appear in any branch or tag.

The pre-rewrite commits are still served by SHA. Please remove these orphaned commits and any cached views of them, and run garbage collection on the repository:

Commits whose tree contains the removed files:
- 53310d037e74994a21d0583e7a291dca27ef8c73
- 1b7b3084af1c3ea4abffd8b3526dc11941e22467
- 90f23ff75f422877d4042c037444197b690d8c75
- ef6d9a811a9b285716d8be8cab1fed3d17d882a5

Their orphaned descendants and siblings from the same rewritten branch:
- 4e3fbaf03a69da667cfde63fc49f21a4f4a94b3b
- 49d8e17dd799efed7c06978abdb94e02fd5514f3
- 092f3e35ee49db0e7c0567a638653f43f9c41066
- 523e2ebbdf4e2f3aa26344d2af5c12ac590f0f63
- afa51fed7695005ff0d2dfc3cea884069f61f7c3
- 60088b5709604cdde44bebac3388fbf51c49ca59
- 5b95ff3df3a121561c920ad75c2d0e6109eca510

The removed paths were:
- workflow-evidence/R001/plane/S1-blocked-worktree.patch
- workflow-evidence/R001/plane/S1b-worktree.patch
- workflow-evidence/R001/plane/S1-invalid-reviewer-outputs/probe_results.json

Other facts:
- The repository has no forks.
- No pull request references these commits. PR #1 was opened after the rewrite and contains only rewritten commits.
- The current branches and tags do not contain the files.

Thank you.
