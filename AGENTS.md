# Version Increment Checklist

When changing Dockerfiles, ALWAYS update the `VERSION` file:

- **PATCH**: typo fixes, broken install corrections
- **MINOR**: adding new tools/packages/variables
- **MAJOR**: removing anything, upgrading Node/OCaml/dune/Alpine/Postgres, changing directory structures

Remember: version must be incremented before pushing.

Keep docs concise and to the point.
