# lscs-claude

A Claude Code plugin for LSCS (dlsu-lscs) engineering work. It gives Claude three things:
- the org's contribution rules;
- safety rules for letting Claude act on GitHub;
- a notebook workflow for working through your assigned tickets across sessions.

## What you get
| Part | What it does |
|---|---|
| Safety rules (always on) | Loaded at every session start, and again after `/clear` and compaction. They say what Claude does alone, what needs your OK first (pushes, PRs, merges, replies to people) and what only humans do (approving PRs, secrets, org settings). The admin bypass is never used. |
| `/lscs-devops:conventions` | The Code Contribution Guidelines and CQA / DevSecOps Standard V2.1.0 in short: branches, commits, PR template, reviews, merge method, CI security, action pinning, images, secrets. Claude loads it by itself when you branch, commit, write a PR, a workflow or a Dockerfile. |
| `/lscs-devops:start` | Session start: reads your `HANDOFF.md`, scans the issues assigned to you, takes in and ranks new ones, checks your PRs for reviews and merges, and reports what's next. |
| `/lscs-devops:pr` | Pre-push check, then a PR proposal you approve before anything is pushed. Review follow-up, then a merge proposal once a qualified reviewer approved. |
| `/lscs-devops:close` | Tests every acceptance criterion live, records the evidence, closes the ticket, writes a test report for the team. |
| `/lscs-devops:end` | Updates `HANDOFF.md` and the ticket notes, gives you a 3-line brief per PR, commits your notes. |

## Install
In Claude Code, in the folder where you work (your notes repo, or a product repo):
```
/plugin marketplace add Enzo-user/lscs-claude
/plugin install lscs-devops@lscs --scope project
```
Project scope keeps the rules out of your non-LSCS projects. Start a new session (or run `/reload-plugins`). To check it loaded, ask Claude "what are the LSCS tiers?": it should answer without reading any files.

## First run: the ticket notebook
1. Make a **private** GitHub repo for your notes (e.g. `<you>/lscs-notes`), clone it, open Claude Code there, and install the plugin as above.
2. Run `/lscs-devops:start`. It asks for your project facts (org, your handle, repos and branch names, reviewers by role, your tools), writes them to `CLAUDE.md`, and creates `HANDOFF.md` and `Tickets/`.
3. Every session after that: `/lscs-devops:start`, work, `/lscs-devops:end`.

Only want the conventions and safety rules? Install it in the product repo and don't use the notebook skills.

## Requirements
- Claude Code with GitHub access: the `gh` CLI (logged in), or the GitHub MCP server.
- Optional: a browser automation tool (issue screenshots, settings pages), and Chrome or Edge to print test reports to PDF.
- Windows: tested with Git Bash. PowerShell is untested; open an issue if the rules don't load.

## Your settings stay yours
The plugin grants no permissions and changes no settings. Claude still asks before any tool you haven't allowed. Your own `CLAUDE.md` can add stricter rules, and they win.

## Updates
`claude plugin update lscs-devops@lscs`, or turn on auto-update under `/plugin` → Marketplaces. Running sessions keep the old version until restarted.

Maintainers: a change reaches installs only when `version` in `plugins/lscs-devops/.claude-plugin/plugin.json` goes up. Check with `claude plugin validate . --strict` before pushing.

## Source and ownership
The rules summarize the LSCS **Code Contribution Guidelines V2.1.0** and **CQA / DevSecOps Engineering Standard V2.1.0** (October 2026), with section numbers so you can check the original. When the standards change, this plugin should too.

Maintained by [@Enzo-user](https://github.com/Enzo-user) for now. The standards prefer team ownership over personal accounts (CCG §17), so the plan is to move this to an LSCS-owned repo. Issues and PRs are welcome.

## License
[MIT](LICENSE): free to use, copy, change and share, for any purpose, as long as the copyright notice stays with it. The license covers this plugin's files; the LSCS standards it summarizes remain LSCS's own documents.
