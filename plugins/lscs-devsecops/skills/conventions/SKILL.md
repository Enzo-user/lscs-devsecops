---
name: conventions
description: LSCS (dlsu-lscs) engineering conventions from the Code Contribution Guidelines and the CQA / DevSecOps Engineering Standard V2.1.0 - branch flow, branch and commit names, PR minimums and template, review ownership, protected branches, merge method, hotfixes, CI and workflow security, action pinning, container images, secrets, evidence, AI-assisted work. Use when branching, committing, writing or reviewing a pull request or issue, or writing a GitHub Actions workflow, Dockerfile or Compose file for an LSCS repository.
---

# LSCS conventions

Summarized from the LSCS **Code Contribution Guidelines V2.1.0** (CCG) and **CQA / DevSecOps Engineering Standard V2.1.0** (CQA), October 2026. Where they differ from the older DevOps Manual v1.1.1 in lscs-infra, they win. Section numbers point to the source: read it when a case isn't covered here. Repo-specific names (the dev branch, who holds each role) are in `## LSCS project facts` in the user's CLAUDE.md.

## Branches (CCG §6, §13; CQA §5)
- working branch → dev → staging → main. Feature, fix, refactor, docs and test work target dev through a PR. dev → staging and staging → main are promotion PRs.
- Never develop on staging or main. A defect found on staging is fixed on a working branch into dev, then promoted again.
- dev is shared integration, staging the release candidate, main the production source.
- dev, staging and main are protected: PRs, relevant status checks, a qualified approval, blocking conversations resolved, no direct or force pushes. main gets the strongest protection.
- Keep working branches short-lived.

## Names and commits (CCG §7)
- Short branch names tied to the issue: `feature/42-google-auth`, `fix/57-expired-session`, `refactor/61-auth-service`, `docs/70-deployment-guide`, `hotfix/92-login-outage`. Use the prefix in the project facts if one is set (e.g. `devops/<N>-<slug>`).
- Commit and squash-merge titles follow Conventional Commits: `<type>[optional scope]: <description>` (feat, fix, docs, refactor, test, ci, chore).
- Name a breaking change in the PR and in the final merge or squash commit, with migration or compatibility notes.

## Pull requests (CCG §8, §9, Appendix A)
- One primary purpose per PR. Open a draft early when architecture, API, database, security or cross-domain feedback would prevent rework.
- Minimums: summary and linked issue, how it was verified, risk or impact, deployment or migration notes when applicable. Template: [pr-template.md](pr-template.md). Repos set up from the older lscs-infra templates lack *Verification* and *Risk / Impact*: add those sections to the body.
- Before requesting review, the author:
  - reads the full diff and removes debug output, dead code, temporary bypasses and accidental files;
  - runs the relevant tests, build, type checks and lint;
  - confirms no secrets or restricted data are committed;
  - checks the acceptance criteria and names known limitations.
- Proof matches the risk of the change.

## Review (CCG §10, §11, §16)
- Frontend and backend PRs: the Project Tech Lead is the primary reviewer.
- DevSecOps-facing PRs: the Senior DevSecOps. Changes to privileged CI/CD workflows **require** the Senior DevSecOps.
- One qualified approval is normally enough. Approval follows expertise and risk, not title. Escalate specialist changes: auth, destructive migrations, secrets, privileged workflows, infrastructure, production access.
- Review labels when useful: BLOCKING, QUESTION, SUGGESTION, NIT.

## Merging (CCG §14, §15)
- Working branch → dev: **Squash and Merge**, unless the project documents a reason to keep the commits.
- dev → staging and staging → main: promotion PRs with **merge commits**. Never squash them.
- Hotfix: hotfix branch → PR to main → critical CI → qualified review → deploy → sync the fix back to staging and dev. A break-glass bypass is limited, recorded (what was bypassed and why) and reviewed afterwards.
- Merge-ready, staging-ready and release-ready are different states. Merge permission and production-deploy permission are separate.

## CI and workflows (CCG §12, §16; CQA §6, §7)
- Every deployable repo has CI where applicable: repeatable install, lint or format, type checks, tests, security checks, production build. Risk tier 2 adds dependency and secret scans and the production image build; tier 3 adds real-database, end-to-end and load evidence (CQA §6).
- GitHub Actions: least-privilege `permissions:`. Never expose production secrets to untrusted pull-request contexts.
- Review new Actions. Pin security-sensitive ones to an exact reviewed commit: `uses: owner/action@<full 40-char sha> # v1.2.3`, with the SHA read from the tag on the day it is pinned. A tag can be moved to other code; a SHA can't. GitHub's own `actions/*` may stay on version tags.
- Fix, replace or remove flaky or ignored required checks.

## Builds and images (CQA §7)
- Commit lockfiles, use repeatable installs, review new dependencies.
- Images: minimal, no secrets or unneeded files, non-root unless documented.
- Exploitable Critical findings: fix, or protect with a verified mitigation, before production. High: fix, mitigation or documented risk acceptance.
- Keep every build traceable to its exact commit; record the commit and image deployed.

## Evidence (CQA §7)
Proof must be reviewable by another officer and tied to the change: the PR or CI run, the issue, the release record. Screenshots support machine-verifiable results; they don't replace them.

## Secrets and access (CCG §16; CQA §8, §12)
- Never commit passwords, API secrets, private keys, tokens, database credentials, production environment contents or restricted member data.
- A credential that reached Git history is compromised: rotate it. Deleting the line is not enough.
- Production secret values never appear in Git, tickets, screenshots, chat or handover documents. Production and service secrets live in Bitwarden (or another approved store); each has a named owner and a revocation path.
- Least access: individual accounts, MFA, scoped service credentials. Org-wide repository or access rules and shared secret changes get Senior or VP review.

## Tooling (CQA §5)
GitHub and GitHub Actions; Docker and Compose; Dokploy + Traefik (deploys, routing); Cloudflare (DNS, edge); Bitwarden (secrets); SigNoz + OpenTelemetry (monitoring); k6 (load tests). Shared hosting, DNS, databases and org accounts go through Systems & Infrastructure; don't create unmanaged production resources.

## Ownership (CCG §17)
The Senior DevSecOps owns repository initialization, branch and ruleset configuration, the CI/CD baseline, environment strategy and deployment controls: those decisions are theirs. Prefer team or role ownership over personal accounts, and document bot or service ownership at handover.

## AI-assisted work (CCG §4, §16; CQA §15)
AI may help, but the contributor must understand, review, test and be able to explain every submitted change. AI-written tests, Dockerfiles, CI workflows and infrastructure get the same review as human ones. No production credentials or restricted data in AI services that are not approved for them.
