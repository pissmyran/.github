# Governance

Rules for every repository under `pissmyran`. Read this file at the start of every session. The owner's instructions always take precedence over this and every other document. Each repository's `AGENTS.md` links here and holds only what is specific to that repository. `CLAUDE.md` in each repository only imports `AGENTS.md`.

- [SERVICES.md](SERVICES.md): extra rules for `dumpen`, `jobb`, `klarsprak` and `politiker`.
- [BASTION.md](BASTION.md): extra rules for `bastion` and the `bastion_*` repositories.

Each agent runs in its own Docker Sandbox without host mounts, with a fine-grained token that reaches only its own repositories.

## Rules

1. **No personal values.** The repositories are public. Never write real IDs, domains, email addresses, names, keys, payment links or account details anywhere, commit messages included. Use placeholders and `example.com`.
2. **No secrets.** Never in Git, logs, issues or documentation. They live outside the repository (Cloudflare, GitHub environments, `wrangler secret put`, the sandbox proxy).
3. **Stay in scope.** Work only in the repositories you were assigned. Never ask for broader token permissions; describe the need in an issue.
4. **Branch and pull request.** Start every session from the latest `main` on its own branch. `main` changes only through a squash-merged pull request. One session, one pull request.
5. **Test before every push.** CI runs no tests. The checks under Commands in the repository's `AGENTS.md` are the test gate. Never push failing tests. Write tests for what you change.
6. **Never touch workflows.** Not `.github/workflows/`, `.github/actions/` or `CODEOWNERS`, even to fix an error. The token cannot push them. Describe the change in an issue. Only Renovate and Dependabot update them.
7. **Review.** CodeRabbit, Sourcery and Codex review, and CodeQL scans. Read the whole review, including threads marked `outdated`. Resolve every thread before merge, replying with what you changed or why the finding is noise. `main` requires a pull request, linear history, resolved threads and CodeQL without errors or serious alerts.
8. **Auto-merge last.** The review bots are advisory: take their findings seriously and weigh them, but they never block a merge, whether or not they have reviewed or have findings. Whether to wait for a bot is a judgment, not a fixed rule. For sensitive or complex changes, wait for a bot that can review soon. For plain documents, merging without waiting is fine, but a review already in progress is let finish so its credits are not wasted. A bot that reports its quota spent for more than about an hour is ignored. Enable auto-merge when all threads are resolved and every check that runs is green.
9. **Fix the cause.** Read everything, find the cause and redo that part properly. Small obvious errors may be fixed directly. Never weaken a test, skip a check or silence a warning.
10. **Security first.** Alerts (CodeQL, Dependabot, secret scanning, Renovate) come before other work. One issue per alert, linked from the fix with `Closes #n`. Never copy a secret into an issue. For an exploitable weakness or a live secret, use a private draft security advisory.
11. **Latest versions.** No new version pins; use `latest` and ranges. Existing pins that Renovate maintains stay, and only Renovate changes them.
12. **Commits and titles.** Use the sandbox's git identity. Never change user, email or signing, and never use `--no-verify`. The pull request title becomes the commit on `main`: Conventional Commits, `type(scope): description`, imperative, specific, under 72 characters. Types: feat, fix, docs, refactor, perf, test, build, ci, chore, style, revert.
13. **Docs in the same pull request.** When behaviour, settings or commands change, update `README.md`, `docs/` and `AGENTS.md`.
14. **No stray branches.** Delete the branch when the pull request is merged or closed. A pull request from a fork is a suggestion: redo the change on your own branch if it is wanted and close the original with thanks and a link.
15. **Zero cost.** Never enable anything that can cost money.
16. **English.** Code, comments, documentation, commits, pull requests, review replies and labels are English. Exceptions are stated in the repository's `AGENTS.md`.
17. **Model before task.** Before every new task, state which model fits best and that it must be activated first. Do not start until the owner has confirmed or switched.

## GitHub access

In the sandbox the token is injected, so `git` and `gh` work as usual. In a cloud session GraphQL is off, so `gh pr` and `gh issue` fail; use `gh api repos/pissmyran/<repo>/…`:

- Open a pull request: `pulls -f base=main -f head=<branch> -f title=… -f body=…`
- Review threads: `pulls/<n>/ccr/review_threads`. Reply: `pulls/<n>/comments/<id>/replies -f body=…`. Resolve: `-X POST pulls/<n>/ccr/comments/<id>/resolve`.
- Checks: `commits/<sha>/check-runs`
- Auto-merge: `-X PUT pulls/<n>/ccr/auto_merge -f merge_method=squash`. If it is refused because the pull request is ready, merge with `-X PUT pulls/<n>/merge -f merge_method=squash`.
- Close: `-X PATCH pulls/<n> -f state=closed`, then `git push origin --delete <branch>`.
- Issues: `issues/<n>` and `issues/<n>/comments`
- Alerts: `dependabot/alerts`, `code-scanning/alerts`, `secret-scanning/alerts`
- GitHub Projects cannot be read or changed.
