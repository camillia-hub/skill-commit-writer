# PR body template

Fill this in from the same diff analysis as the commit message.

```markdown
## Summary
<one to three sentences: what this PR does and why>

## Changes
- <notable change, one per line, reference files when useful>

## Testing
- [ ] Unit tests added/updated
- [ ] Manually verified: <what was clicked or run>

## Breaking changes
<"None" or one bullet per breaking change with migration notes>

Refs: <ticket-id>
```

Guidance:

- Keep the body under roughly 40 lines; link to design docs instead of pasting them.
- The Testing section must reflect what actually happened; uncheck the boxes that were not done.
- If the repository has its own `.github/PULL_REQUEST_TEMPLATE.md`, use that structure instead and merge these sections into it.
- Screenshots belong in the body for UI changes; drag them in or reference the paths the user provided.
