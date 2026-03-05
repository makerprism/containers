# Agent Guidelines for Containers

## Communication Style

Be concise. Get to the point. No fluff, no redundant explanations. Say what needs saying and stop.

**Always link PRs.** When mentioning PRs, include the full GitHub URL. Never just say "PR #123" — always say "PR #123: https://github.com/owner/repo/pull/123"

# Version Increment Checklist

When changing Dockerfiles, ALWAYS update the `VERSION` file:

- **PATCH**: typo fixes, broken install corrections
- **MINOR**: adding new tools/packages/variables
- **MAJOR**: removing anything, upgrading Node/OCaml/dune/Alpine/Postgres, changing directory structures

Remember: version must be incremented before pushing.
