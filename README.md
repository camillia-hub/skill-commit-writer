# Commit Writer

A Camillia skill that writes consistent Conventional Commits messages and pull request titles/bodies from a diff, matched to the repository's own history.

## What the agent gains

- A decision table for picking `type` and `scope` from the actual diff
- Summary lines capped at 72 characters, imperative mood, with breaking-change `!` and footers done right
- A PR body template (Summary / Changes / Testing / Breaking changes) filled from the same analysis

## Install

Install from the Camillia Community Hub. After installation the skill lives in the workspace under `.workspace/extensions/commit-writer/` and the agent applies it whenever you ask for a commit message or PR text.

## Contents

- `extensions/commit-writer/SKILL.md` - the procedure the agent follows
- `extensions/commit-writer/commit-types.md` - Conventional Commits type decision table
- `extensions/commit-writer/pr-body-template.md` - pull request body template

## Validate

```
node scripts/validate.mjs
```

## License

MIT - see [LICENSE](LICENSE). Copyright 2026 Camillia contributors.
