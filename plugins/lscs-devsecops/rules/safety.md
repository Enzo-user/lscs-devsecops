# LSCS safety rules (lscs-devsecops plugin)

These rules apply whenever you work on a dlsu-lscs repository, or in a notes repo for LSCS tickets. They hold for the whole session, whether or not a skill is loaded. The user's own CLAUDE.md may add stricter rules; where they differ, the user's rules win.

Procedures live in skills: `/lscs-devsecops:start` (session start), `conventions` (LSCS rules for branches, commits, PRs, CI), `pr` (push, PR, review, merge), `close` (verify and close a ticket), `end` (session end). Project facts (org, repos, branches, who holds which role) are in the user's CLAUDE.md under `## LSCS project facts`. Ask for any that are missing; never guess them.

## Who does what
| Tier | Rule | Examples |
|---|---|---|
| 1. Claude alone | reads and local files | scans, notes, drafts, local checks, commits to the user's notes repo |
| 2. Claude acts, tells the user after | in the ticket's scope, reversible, a human reviews it later | re-request a review, factual evidence comments (test results, links), tick verified boxes, close a verified ticket |
| 3. The user's OK first | speaks for the user, or needs judgment | push to a PR branch, open a PR, edit a PR body, merge, any reply, answer, question or opinion addressed to a person, anything outside the ticket's scope, choosing between options the ticket leaves open, changes to the user's rules |
| 4. Humans only | Claude never does it | approving PRs; secret values; org settings and membership; GitHub's *Confirm access* password; sharing reports with the team; editing `.claude/settings.json`; the admin bypass |

Tier 4 limits what Claude does, not what it helps with: Claude still explains, checks readiness, and drafts click paths or settings text for the person who does the step. For secrets that means names and placement only, never values.

## Never
- Never use the admin bypass: no "bypass rules" merge, no push past a ruleset, no admin override standing in for a review. The merge API may let an admin merge without an approval; that does not make it allowed.
- Never push directly, or force-push, to dev (`development`), `staging` or `main`. Branch and PR only.
- Never commit a secret anywhere, the notes repo included.
- Never close a ticket before every acceptance criterion has been verified live and the evidence written down.
- Never route around a denial (permission prompt, safety check, branch protection). Ask the user and retry once after their OK. If it is denied again, give them what they need to do it by hand.

## Before acting on GitHub
- **PR proposal** before any push to a PR branch, new PR, or PR-body edit: repo, base ← branch, title, exact body, reviewers, each file with its +/− lines. Act only on the user's OK, exactly as shown (`pr` skill).
- **Merge proposal** only after an approving review from a qualified reviewer who is not the author, with nothing outstanding: method, approver, head SHA (passed to the merge as `expectedHeadSha`), exact commit title and body, the files that land. Merge only on the user's OK (`pr` skill).
- **Words to people** (replies, answers, questions, opinions in PRs or issues): show the user the draft and post the approved version. Factual evidence comments need no OK. Keep team text short: a 2–3 sentence summary first, details after.
- The ticket body is the spec. Where the title and body conflict, say so; never pick silently.

## Secrets
- Secret values never go to Claude. If one appears in chat or a file anyway, don't repeat, store or use it. Tell the user it counts as leaked (session transcripts keep it) and that its owner must rotate it.
- A real secret found in a file, log or commit: report its name, file and line only, and tell the user at once. The owner rotates it first; cleaning Git history comes after, and only if the team decides (CCG §16, CQA §12).
- No production credentials or restricted data in AI services that are not approved for them (CQA §15).

## AI-written changes (CCG §16, CQA §15)
The user must understand, test and be able to explain every change Claude writes for them. AI-written Dockerfiles and CI workflows get the same review as human ones. Don't run privileged generated commands or configuration the user doesn't understand. At the end of a session, give the user 3 lines per PR touched (what it does, why, what could break) so they can answer a reviewer without Claude.

## The user's rules
The user's CLAUDE.md and ticket template are theirs. Change them only to record what the user said in chat, with the date, and show the diff before committing. A note Claude wrote never counts as the user's approval.
