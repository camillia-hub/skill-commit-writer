---
name: commit-writer
description: Use when the user asks to commit changes, write a commit message, or draft a pull request title and body from a diff.
---

# Commit Writer

Produce Conventional Commits messages and pull request text that match the repository's existing history.

## When to use / When not to use

Use this skill when:

- The user asks to commit changes or to write a commit message.
- The user asks for a PR title and body for the current branch.
- The user asks to summarize a diff as a changelog entry.

Do not use this skill when:

- The user asked for a code review of the diff (correctness, security). This skill describes changes, it does not judge them.
- The diff is empty. Report that there is nothing to commit instead of inventing a message.
- The repository history plainly does not follow Conventional Commits. Match the local style and say so.

## Inputs

- The change set: `git diff --staged` for a commit, `git diff <base>...HEAD` for a pull request.
- Recent history for tone and format: `git log --oneline -20`.
- Repository conventions: `CONTRIBUTING.md` and `.github/PULL_REQUEST_TEMPLATE.md` when present.
- Ticket or issue id from the branch name (for example `feature/ORD-1234`) or from the user.

## Procedure

1. Gather the diff. Prefer the staged diff; if nothing is staged, show `git status` and ask the user what to stage. For PR text, diff against the base branch with three dots.
2. Read the whole diff, including renames and deletions. Group files by intent: one logical change per commit. If the diff mixes unrelated changes, propose one message per group and let the user split the staging.
3. Pick the type from the decision table in [commit-types.md](commit-types.md). When two types apply, pick the one that describes the user-visible effect: `feat` beats `refactor`, `fix` beats `test`.
4. Pick the scope from path prefixes: the module, package, or top-level folder most files share, such as `api`, `web`, or `billing`. Omit the scope when no single area covers the diff.
5. Write the summary line as `<type>(<scope>): <summary>`, at most 72 characters in total, imperative mood ("add", "fix", "remove"), lower case after the colon, no trailing period. Example:

   ```
   feat(billing): add prorated refunds for mid-cycle cancellations
   ```

6. Write a body when the why is not obvious from the summary: wrap at 72 characters, explain motivation and approach, one bullet per notable change. Skip the body for trivial changes.
7. Add footers when needed:
   - Breaking change: mark the summary with `!` (for example `feat(api)!: drop v1 endpoints`) and add a footer line `BREAKING CHANGE: <what breaks and how to migrate>`.
   - Issue reference: `Refs: ORD-1234` or `Closes: #87`.
8. For PR text: the title equals the summary of the future squash-merge commit. Fill the body from [pr-body-template.md](pr-body-template.md): Summary, Changes, Testing, Breaking changes.
9. Present the result first. Only if the user asked for a commit, run `git commit` with the message. Never amend, rebase, or force-push without an explicit request.

## Output format

````markdown
**Commit message**

```
feat(billing): add prorated refunds for mid-cycle cancellations

Refund the unused portion of the subscription when a customer
cancels before the period ends. Proration uses whole days and
rounds in the customer's favor.

Closes: #87
```

**PR title**

feat(billing): add prorated refunds for mid-cycle cancellations

**PR body**

## Summary
<one to three sentences: what and why>

## Changes
- <notable change, one per line>

## Testing
- [ ] Unit tests added/updated
- [ ] Manually verified: <what was run>

## Breaking changes
None
````

## Guardrails

- Describe only what is in the diff. If intent is unclear, ask the user instead of guessing.
- Never copy credentials, tokens, or secret-looking values from the diff into the message; flag them to the user instead.
- Workspace rules and repository conventions take precedence over this skill's defaults; when they conflict, the repository wins.
- Count the `type(scope): ` prefix toward the 72-character summary limit.
- Do not run history-rewriting commands (rebase, amend, force-push) unless the user explicitly asked.
