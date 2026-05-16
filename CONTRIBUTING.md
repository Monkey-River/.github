# Contributing to Monkey & River Projects

This document defines how we use Git and GitHub across all Monkey & River engagements. These conventions apply to every repository under the Monkey-River organisation and should be followed on client repositories unless the client has an established workflow that takes precedence.

---

## Branching Strategy

We use a four-tier branching model. Code flows upward through each environment in order. No tier is ever skipped.

```
main (production)
  └── staging (pre-production / QA)
        └── development (integration)
              └── feature branches (individual work)
```

**Rules**

- Never commit directly to `main`, `staging`, or `development`. All changes flow through pull requests.
- Feature branches are created from `development` and merged back into `development`.
- `development` is merged into `staging` when the team is ready for a QA cycle.
- `staging` is merged into `main` for a production release.
- If a hotfix is required in production, branch from `main`, apply the fix, merge into `main`, and then cascade the fix down into `staging` and `development` to keep all branches in sync.

---

## Branch Naming

Feature branches follow this format:

```
[name]/[ticket-id]-[short-description]
```

Examples:

```
rynhardt/DEV-142-user-registration-flow
thomas/DEV-87-fix-payment-timeout
edwin/DEV-203-add-test-builder-export
```

Keep descriptions short (3 to 5 words), lowercase, and hyphen-separated. The ticket ID ties the branch to the ClickUp task for traceability.

---

## Commits

### Commit Message Format

Write clear, descriptive commit messages. Use the imperative mood (e.g. "Add validation" not "Added validation").

```
[type]: [short description]

[optional body - explain why, not what]
```

**Types**

| Type | When to use |
|---|---|
| `feat` | A new feature or capability |
| `fix` | A bug fix |
| `refactor` | Code restructuring with no behaviour change |
| `docs` | Documentation only |
| `test` | Adding or updating tests |
| `chore` | Tooling, config, or dependency changes |

**Examples**

```
feat: add UUID generation for own-participant test links
fix: handle not-found error when fetching completed tests
refactor: consolidate test group participant update logic
docs: add runbook for staging deployment
```

### Commit Hygiene

- Commit often, but keep each commit focused on a single change.
- Do not commit commented-out code, debug logs, or unrelated changes.
- Squash or clean up your commits before opening a PR if the history is messy.

---

## Pull Requests

### Before You Open a PR

- Ensure your branch is up to date with `development`. Rebase or merge `development` into your branch and resolve any conflicts.
- Verify the build passes locally.
- Review your own diff before submitting. Catch the obvious issues yourself.

### PR Template

Every pull request must follow the template below. It is configured as the default PR template in each project repo.

```markdown
## Ticket
- [Link to ClickUp ticket or GitHub issue]

## What was done
[Short description of what this PR achieves and what changes were made.
Use a bullet list for multiple changes, grouped logically.]

## Relies On
<!-- Remove this section if not applicable -->
- [Link to dependent PR] - [Status: Open / Merged]

## Unseen Changes (DB, Config, Policy)
<!-- Remove this section if not applicable -->
- [List any database migrations, config changes, environment variable additions,
  policy changes, or other changes not visible in the application code]

## Version
<!-- Remove this section if not applicable -->
[What version does this PR apply to]
```

### Filling Out the Template

**Ticket** - Always link to the ClickUp task or GitHub issue. If the work spans multiple tickets, list all of them.

**What was done** - Describe the changes at a level that a reviewer can understand the intent before reading the code. Group related changes together. Be specific. "Fixed some bugs" is not acceptable.

**Relies On** - List any other PRs that must be merged before or alongside this one. Include the branch name or PR link and note whether it has already been merged.

**Unseen Changes** - Flag anything a reviewer or deployer would not see by reading the application code. Database migrations, new environment variables, config file changes, IAM policy updates, or third-party service changes all belong here.

**Version** - If the project uses versioning, state which version this PR targets.

### After Opening a PR

- Set a reviewer. Every PR requires at least one reviewer.
- Respond to review feedback promptly. If you disagree with a comment, discuss it.

---

## Code Reviews

### As a Reviewer

- Review within 24 hours of being assigned. If you cannot, let the author know so they can reassign.
- Focus on logic, correctness, and maintainability. Do not nitpick formatting if the project has a linter.
- If you request changes, be specific about what needs to change and why.
- Approve when you are satisfied. Do not leave PRs in limbo.

### As an Author

- Keep PRs small and focused. Large PRs are harder to review and more likely to introduce issues.
- If a PR is large by necessity, include a summary that guides the reviewer through the changes in a logical order.
- Treat review comments as a conversation, not criticism.

---

## Release and Deployment Flow

1. **Feature complete in `development`** - All feature branches for the release are merged and tested together in the development environment.
2. **Merge `development` into `staging`** - Create a PR from `development` to `staging`. QA, client review, and final validation happen here.
3. **Merge `staging` into `main`** - Once staging is approved, create a PR from `staging` to `main`. This triggers the production deployment.

Specific deployment procedures, approval gates, and rollback processes vary by project. Refer to your project's operations documentation for the details.

---

## Quick Reference

| Action | Rule |
|---|---|
| Branch from | `development` (features) or `main` (hotfixes) |
| Branch naming | `name/TICKET-id-short-description` |
| Commit style | `type: imperative description` |
| PR reviewer | At least one required |
| PR review SLA | Within 24 hours |
| Direct commits to `main` / `staging` / `development` | Never |
