
# Copilot instructions

## Commit message generation

When generating commit messages for changes made in this repository, follow these rules:

- Write all commit messages in English.
- Use Conventional Commits format:
  `<type>(<scope>): <imperative description>`
- Allowed types: feat, fix, docs, refactor, test, build, ci, chore.
- Keep the subject concise, ideally under 72 characters.
- Describe the actual change, not merely the filename.
- Use imperative mood.
- Do not use generic subjects such as "Update file", "Changes" or "Fix".
- Add a body only when additional context is useful.
- Do not invent changes that are not present in the diff.

## Pull request titles

When describing a change that contains one logical change, use the same
Conventional Commits format for the pull request title.

The title must summarize the change rather than the act of committing it.
