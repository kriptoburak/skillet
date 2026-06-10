---
name: pr-description
description: Write a reviewer-first pull-request description from the actual branch diff. Use when opening a PR or asked to write/improve a PR description.
version: 1.0.0
license: MIT
keywords: [git, pull-request, review, workflow, github]
---

# pr-description

The reader is a reviewer deciding *how to review*, not a historian. Write from
the diff, not from memory of the conversation.

## Procedure

1. Establish the real change set: `git log main..HEAD --oneline` and
   `git diff main...HEAD --stat`. Read the diff of every file a reviewer will
   question.
2. Open with **one sentence of why** — the problem or goal, not the activity
   ("Checkout fails for SSO users" beats "Update auth flow").
3. **What changed**, grouped by intent rather than by file: behavior changes
   first, then refactors, then mechanical churn (renames, formatting) called
   out explicitly so reviewers can skim past it.
4. **How to verify**: the exact commands/steps you ran and their result. If CI
   covers it, say which jobs; if manual, give the click-path.
5. **Risk and rollout**: what could break, feature flags, migrations, revert
   plan. One line each. Omit the section only if genuinely none.
6. Call out anything you want focused review on ("the locking in store.js is
   the risky part") — reviewers allocate attention where the author points.

## Anti-patterns

- Restating the commit list — the PR already shows it.
- "See ticket" as the why — summarize the ticket in one line, then link it.
- Screenshots without a caption saying what to look at.
- Describing the diff file-by-file — group by intent.

## Skeleton

```markdown
Fixes checkout failing for SSO users (#412).

**What**
- Treat IdP-initiated sessions as authenticated in the cart guard (behavior)
- Extract session checks into `session.ts` (refactor, no behavior change)

**Verify**: `npm test` (all green) + manual SSO login → add to cart → checkout.

**Risk**: session guard touches every authed route; revert is a single commit.
Focus review on `session.ts:40-75`.
```
