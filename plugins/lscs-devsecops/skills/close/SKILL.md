---
name: close
description: Verify and close an LSCS ticket after its pull request is merged - test every acceptance criterion live, record machine-verifiable evidence, tick the boxes, post the evidence comment, close the issue and write a test report for the team. Use when a ticket's PR has merged, or when the user asks to verify, resolve or close a ticket.
---

# Resolving a ticket

Applied is not resolved. A ticket closes only when every acceptance criterion has been tested live, after the merge, on the branch the ticket names.

1. **Test each acceptance criterion** against the merged result: run the workflow on the target branch, call the API, open the page. A failure keeps the ticket open: note it, fix it (`pr`), re-test.
2. **Evidence, machine-verifiable first** (CQA §7). Each criterion cites the run URL, commit SHA or API output that proves it.
   - A criterion with a UI also gets a screenshot when a browser tool exists: `evidence/<REPO>-<N>-<item>.png`, shown to the user with one or two sentences on what it proves.
   - Config facts with no UI (a token's scope) are cited instead.
3. **Write it down:** the ticket MD's *Verification* table (criterion, evidence with date, screenshot) and its closing checklist.
4. **On GitHub** (Tier 2: act, then tell the user):
   - tick the boxes on the issue;
   - post one evidence comment: a 2–3 sentence summary first, then one line per criterion with its link;
   - close as completed. A PR into a non-default branch doesn't auto-close its issue, so close it by hand;
   - delete any throwaway test workflow the ticket used.
5. **Test report**, written for the devs, not for Claude. Copy [test-report.html](test-report.html) to `evidence/<REPO>-<N>-test-report.html` and fill it in: issue link, what changed, the exact commit tested, each criterion with its evidence, screenshot and a plain-language line, the date, who verified. Screenshots go in by relative path.
   If a Chromium browser is installed, print it to PDF. A separate `--user-data-dir` avoids clashing with a running browser:
   `<chrome or msedge> --headless --disable-gpu --no-pdf-header-footer --virtual-time-budget=5000 --user-data-dir="<temp dir>" --print-to-pdf="<abs>/evidence/<name>.pdf" "file:///<abs>/evidence/<name>.html"`
   Check that the PDF opens and shows the images, then commit both. The user shares the report with the team.
6. **Update the notes:** the queue row (CLOSED, date, report) and the MD's status line.
