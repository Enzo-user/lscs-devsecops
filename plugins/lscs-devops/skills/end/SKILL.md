---
name: end
description: End an LSCS DevSecOps ticket session - update HANDOFF.md and the ticket notes, brief the user on every pull request touched, show any rule-file diff, then commit and push the notes repo. Use when the user wraps up ("end", "stop here", "that's it for today") or before the context runs out.
---

# Session end

1. **HANDOFF.md:** ticket queue, *Last ticket scan* and *Last PR check* if they ran, next action, to-dos, open questions. Add a session-log entry, newest first, numbered by HANDOFF's own count, with date and topic (`### Session 7 — 2026-10-13 (CI review)`). Every session gets one, also a session with no ticket work.
2. **Ticket MDs** worked on: status and priority lines, *Needs before start*, new facts with their dates, the log.
3. **PR brief** in chat: for every PR opened or changed this session, 3 plain lines: what it does, why, and what could break. The user owns everything posted under their name and should be able to answer a reviewer without Claude.
4. **Rule files** (the user's CLAUDE.md, `Tickets/TEMPLATE.md`) changed this session: show the user the diff and wait for the OK before committing them.
5. **Commit and push** the notes repo. Nothing in it is secret: secret names only, never values.
