# <REPO> #<N> — <title without the prefix> (<owner/repo>)

**Status:** TODO · intake YYYY-MM-DD (Session N) · <one line: where it stands, what happens next>
**Priority:** <rank> of <open tickets> · <the rule that placed it, e.g. "3. dependency: <other ticket> needs this"> · checked YYYY-MM-DD
**Issue:** <url> · by <author> · assigned <assignees> · labels <labels> · type <type> · milestone <milestone or none> · created YYYY-MM-DD
**Screenshot:** `<REPO>-<N>-<slug>.png` (taken YYYY-MM-DD, or pending)
**Sibling:** <the same ticket in another repo, whose MD lists differences only; or none>
**Delivery:** files in `deliverables/<repo>/…` (or: settings only, no files) · branch `<branch>` off `<base>` · PR proposal OK'd YYYY-MM-DD · PR <url, or not opened>

## What the ticket asks
<Body, verbatim: quote it, the body is the spec. Keep links. Note any title/body conflict here; never resolve it silently.>

**Acceptance criteria (verbatim from the issue):**
- [ ] …

## Needs before start
One row per need. Delete the kinds that don't apply, but never leave the table empty: write "none found" with the date checked.

| Kind | Need | Owner | Met? | Checked |
|---|---|---|---|---|
| Access | <role needed vs mine> | <me> | ☐ | YYYY-MM-DD |
| Secret / token | <which (name only), who creates it, expiry> | | ☐ | |
| Decision | <question only a person can answer> | <who> | ☐ | |
| Dependency | <ticket or PR that must land first> | | ☐ | |
| Approval | <required review: Senior DevSecOps for CI/CD workflows (CCG §16), else per CCG §10> · PR and merge proposals need my OK · pre-approved: <steps (YYYY-MM-DD), or none> | | ☐ | |
| Tool limit | <browser-only step, password prompt, what this machine can't run> | | ☐ | |
| Deferral | <who deferred it, when> | | ☐ | |

## Verified current state (YYYY-MM-DD)
<Live facts from GitHub that the plan relies on, each with its date.>

## Diagnosis and open questions
TODO

## Plan
TODO. Numbered steps; mark each step's owner (Claude, <me>, team).

## Verification
| Acceptance item | Evidence (run / commit / API URL, date) | Screenshot (`evidence/`) |
|---|---|---|

Closing checklist (`/lscs-devops:close`):
- [ ] PR merged by <who> · YYYY-MM-DD (if by Claude: my OK on the merge proposal, date)
- [ ] every acceptance item tested live; evidence above; screenshots shown to me
- [ ] boxes ticked on the issue · evidence comment <url>
- [ ] closed as completed (or by its PR) · YYYY-MM-DD
- [ ] `evidence/<REPO>-<N>-test-report.pdf` built and checked; shared with the team

## Log
- YYYY-MM-DD: intake (Session N).
