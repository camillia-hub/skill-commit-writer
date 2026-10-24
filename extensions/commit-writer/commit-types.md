# Conventional Commits type decision table

Pick exactly one type per commit. Read the diff, not the branch name.

| Type | Use when the diff... | Example |
|---|---|---|
| feat | adds or changes user-facing behavior | `feat(search): support phrase queries in quotes` |
| fix | corrects incorrect behavior | `fix(auth): refresh tokens before the expiry check` |
| perf | improves performance without changing behavior | `perf(list): virtualize the orders table` |
| refactor | restructures code without changing behavior | `refactor(api): split order service out of the controller` |
| docs | changes documentation only | `docs: document the webhook retry policy` |
| style | formatting, whitespace, lint fixes only | `style: apply prettier to the billing module` |
| test | adds or fixes tests only | `test(cart): cover discount stacking rules` |
| build | changes the build system or dependencies | `build: bump vite to 6.x` |
| ci | changes CI configuration | `ci: cache node modules between jobs` |
| chore | maintenance that fits nowhere else | `chore: remove deprecated feature flags` |
| revert | reverts a previous commit | `revert: feat(search): support phrase queries` |

Tie-breakers:

- `feat` and `fix` are the only types that imply a version bump (minor / patch). A `BREAKING CHANGE` footer implies major.
- Between `feat` and `fix`: did the previous behavior already promise this? Yes -> `fix`. No -> `feat`.
- A dependency bump that fixes a vulnerability is `fix(deps)`, not `build`.

## Breaking-change footer

Format:

```
feat(api)!: drop the v1 users endpoint

BREAKING CHANGE: GET /v1/users is removed. Migrate to GET /v2/users,
which returns fullName instead of firstName/lastName.
```

The `!` after the type/scope is required together with the footer.
