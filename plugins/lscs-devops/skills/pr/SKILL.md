---
name: pr
description: Push a branch, open or edit a pull request, answer a reviewer, or merge in an LSCS repository - the pre-push check, the PR proposal the user approves before anything is pushed, review follow-up, and the merge proposal. Use before any git push to an LSCS repo, before creating or editing a PR, before replying on a PR, and before merging one.
---

# Pull requests: propose, push, follow up, merge

Every step here that writes to GitHub is Tier 3 in the safety rules: it happens only after the user's OK on exactly what will be sent.

## Drafting
- Draft files in the notes repo under `deliverables/<repo>/`, mirroring that repo's root. The notes repo is what survives between sessions.
- To test or ship, clone the target branch into a temp folder, copy the files in, and run every check this machine can (build, lint, tests, YAML parse). What it can't run goes in the ticket's *Needs* as a tool limit, or into a throwaway `workflow_dispatch` workflow in the notes repo, deleted when the ticket closes.

## 1. Pre-push check
- Re-verify the live base branch: it exists, its protection is as expected, and you know its latest commit.
- The diff holds only files the ticket asks for. Every line traces to a scope item or an acceptance criterion; nothing extra rides along.
- Read the full diff as its author (CCG §9): no debug output, dead code, temporary bypasses or stray files.
- Syntax-validate what this machine can, and scan the diff for secrets.

## 2. PR proposal: show the user, wait for the OK
- **Repo, and base ← branch.** Base: the one the ticket names, else the dev branch (`conventions`). Branch: as the project facts say, else `<type>/<issue>-<slug>`.
- **Title:** Conventional Commits.
- **Exact body:** the repo's PR template, covering the CCG §8 minimums (summary and linked issue, how it was verified, risk/impact, deployment notes). Missing sections: take them from [pr-template.md](../conventions/pr-template.md). *Notes for reviewers*: 5 bullets or fewer. Use `Refs #N` instead of `Closes #N` when the merge should not close the issue.
- **Reviewers:** from the project facts, per `conventions`. CI/CD workflow changes need the Senior DevSecOps.
- **Files:** each file with its +/− lines.

On the OK, push and open the PR exactly as shown. If a tool can't push to the org repo, commit in the clone and run `git push -u origin <branch>` from there.

## 3. Review follow-up (at every start-of-session PR check)
- Changes requested, or questions: fix on the same branch, re-run the checks, update the evidence. Show the user the fix together with the drafted reply. Push and post on the OK, then re-request review.
- A PR-body edit: show the new text first.
- No review after 2 working days: a nudge on the user's to-dos. No reminder comments.

## 4. Merge proposal: only when the PR is ready
Ready means: an approving review from a qualified reviewer who is not the author, no "changes requested" outstanding, every question answered or fixed, required checks green. Then show the user:
- **PR, base and method:** squash for a working-branch PR, merge commit for promotion PRs (dev → staging → main) (CCG §14). Claude doesn't open promotion PRs unless asked.
- **Approval:** who approved and when; open threads (none); checks.
- **Head SHA,** passed to the merge as `expectedHeadSha`. A push after the OK makes the merge fail, and Claude asks again.
- **Exact commit title and body.** Squash: the PR title plus ` (#N)`, with the PR's summary bullets as body; name any breaking change (CCG §7). Merge commit: GitHub's default, `Merge pull request #N from <owner>/<branch>`.
- **What lands:** each file with its +/− lines, and one line on what it does.

On the OK, merge with exactly those values. If a safety check denies it: ask the user, retry once. Denied again: give the user the title and body to merge by hand. A reviewer may also merge on their own; then record who and when.

Never the admin bypass, even when the API would let an admin merge without an approval.

After the merge: `close`.
