---
name: start
description: Start an LSCS DevSecOps ticket session from a notes repo - read HANDOFF.md, scan the GitHub issues assigned to the user, take in and rank new tickets, check the user's open pull requests for reviews and merges, then report what is new and what to work on. Sets up the notes repo on first run. Use at the beginning of a session, when the user says "start" or "continue", or before picking the next ticket.
---

# Session start

Run steps 1–5 every session, also when the user's prompt names other work. If a step can't run, or the user says to skip it, say which and why, and note it in the session log. This skill only reads GitHub; it writes nothing there.

## 0. First run
- `CLAUDE.md` has no `## LSCS project facts` section: ask the user for every line of the block below in one message, then write it into `CLAUDE.md`. Never guess a value.
- No `HANDOFF.md`: create it from [handoff.md](handoff.md). No `Tickets/`: create it and copy [ticket.md](ticket.md) to `Tickets/TEMPLATE.md`.
- Not a git repo: suggest `git init` and a private GitHub repo for the notes. The notes are what survives between sessions.

```
## LSCS project facts
- Org: dlsu-lscs · Me: <GitHub handle> · My role: <admin | write | ...> on <repos>
- Repos: <owner/repo> (dev `<development | dev>`, staging `staging`, default `main`); ...
- Reviewers: Senior DevSecOps <handle>; Project Tech Lead <handle>; <other role> <handle>
- Tickets: title prefix <[DEVOPS]> · issue type <Task> · branch names <devops/<N>-<slug>>
- Tools: GitHub via <gh | GitHub MCP> · browser tool <name | none> · can't run here: <Docker, ...>
```

## 1. Read HANDOFF.md
Top to bottom. Facts carry the date they were checked; treat older ones as stale until re-checked.

## 2. Ticket scan
Search open issues with `is:open assignee:<me> org:<org>` (`gh search issues`, or the GitHub MCP `search_issues`). Compare the results with the HANDOFF queue.

**Not in the queue: intake it.** Only reads on GitHub and local files.
1. Screenshot the issue page if the facts list a browser tool: `Tickets/<REPO>-<N>-<slug>.png`. Otherwise quote the body in the MD and mark the screenshot pending.
2. Copy `Tickets/TEMPLATE.md` to `Tickets/<REPO>-<N>-<slug>.md`. Fill in the issue facts, the body and acceptance criteria verbatim, *Needs before start*, and the priority with its reason. Diagnosis and plan may stay TODO.
3. The same ticket filed in two repos (same title, author and day): a full MD for one, differences only in the sibling.
4. Add the row to the queue.

**In the queue, updated since the last scan:** read the issue and its comments, log what changed in its MD, and re-check its needs.

**In the queue but missing from the results:** someone closed or unassigned it. Find out which, then update the row and the MD.

Set *Last ticket scan* in HANDOFF: UTC time, number found, what was new or changed.

## 3. Rank
Apply in order; the first rule that separates two tickets decides.
1. **The user's order**, recorded as `<me> override YYYY-MM-DD`. It stands until the user changes it.
2. **Urgent:** it blocks a teammate right now, or the user or the author calls it urgent.
3. **Dependency:** a ticket another queued ticket needs comes first.
4. **Team signals:** milestone or due date, priority labels, the project board's priority field.
5. **Oldest first.**

Deferred tickets keep their row, unranked, with who deferred them and when. A ticket that moves gets the reason on its *Priority* line. A ticket waiting on someone keeps its rank; the session moves on to the next one that can start.

**Needs before start**, one row each in the ticket MD (owner, met or unmet, date checked):
- access: the role it needs vs the user's;
- secrets and tokens: which (names only), who creates them, when they expire;
- decisions only a person can answer, and who;
- dependencies: tickets or PRs that must land first;
- approvals: the reviews required (`conventions`), and the steps that need the user's OK;
- tool limits: browser-only settings, password prompts, what this machine can't run;
- deferral: who deferred it and when.

## 4. PR check
Find the PRs: `is:open author:<me> org:<org>`, plus every PR the queue lists, even if the search misses it (it may have been merged or closed). For each, read its state, merged flag, mergeable state, reviews, review comments and comments, and compare with its ticket MD. Record the outcome in the MD and the queue row:
- **Approved** (by a qualified reviewer who is not the author, nothing outstanding): record who and when, and prepare the merge proposal (`pr` skill).
- **Changes requested, or new questions:** quote them in the MD's log. The next step is Claude's: fix, draft the reply, re-test.
- **Merged:** record who and when. Next: `close`.
- **Closed without merge:** record who and when, and ask the user. Don't reopen it.
- **No review yet:** one log line. Two working days after the review was requested (or last re-requested), add "nudge <reviewers> on <PR>" to the user's to-dos. Claude posts no reminder comments.

An open PR by the user that no ticket MD names: note it in HANDOFF and ask. Set *Last PR check* in HANDOFF.

## 5. Report and pick
Tell the user in a few lines what is new (tickets and PRs), what is at the top of the queue, and for each blocked ticket what it waits for and on whom. Then pick the work:
- what the user's prompt names;
- if they only said "continue", HANDOFF's *Next action*;
- otherwise the highest-ranked ticket whose needs are all met.

If nothing can start, say so and stop. Read only the picked ticket's MD (and its sibling's), and re-verify live state before acting: is the issue still open, the branch still as expected, the secret still there?
