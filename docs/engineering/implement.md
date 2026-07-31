Quickstart:

```bash
npx skills add mattpocock/skills --skill=implement
```

```bash
npx skills update implement
```

[Source](https://github.com/mattpocock/skills/tree/main/skills/engineering/implement)

## What it does

`implement` builds one or more authorized Repository Deliveries from a spec or tickets. It records an exact review base for every repository, drives each settled seam through test-driven development, commits focused work, runs repository-owned validation, and returns evidence bound to exact local heads.

The defining constraint is authority. A standalone invocation may own the ticket's complete one-to-many Repository Scope, while a Coordinator-narrowed invocation may change exactly one named Repository Scope entry in one supplied Execution Worktree. The narrowed form cannot start reviewers, publish, or change tracker state.

## When to reach for it

You invoke this by typing `/implement` — the agent won't reach for it on its own.

Reach for it once the work is settled and ready to build, whether the ticket changes one repository or several. A Coordinator can use the same public skill to give one Implementation Agent a complete repository-specific assignment. For a concrete behavior that only needs a test-first loop, use [tdd](https://aihero.dev/skills-tdd) directly.

## Prerequisites

The assignment needs authoritative behavior and settled seams. Federated work also needs explicit Repository References, a non-empty writable Repository Scope, repository-specific outcomes and validation obligations, and isolated writable worktrees. Read-only context repositories do not become writable scope.

## One model, two modes

A single-repository build is the one-element case of the same **Repository Delivery** model used for multiple repositories. Standalone mode validates every delivery and closes with one [code-review](https://aihero.dev/skills-code-review) invocation over the complete fixed target set.

Coordinator-narrowed mode is deliberately smaller. It validates the supplied worktree and exact launch commit, changes only that repository, commits and validates locally, then returns the fixed base, exact local head, commands, outcomes, and blockers. Material scope or seam contradictions go back to the Coordinator instead of being resolved by expanding the assignment.

## It's working if

- Every changed repository has its own fixed review base, exact local head, focused commits, clean delivery state, and validation evidence.
- A changed head invalidates its earlier validation and Standards evidence.
- A narrowed assignment returns one repository result and performs no review, publication, Review Proposal, or Work Tracker mutation.
- A standalone multi-repository assignment ends with repository-local Standards review and one whole-bundle Spec review.

## Where it fits

`implement` is the build step near the end of the main chain:

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

It drives [tdd](https://aihero.dev/skills-tdd) inside each Repository Delivery and uses [code-review](https://aihero.dev/skills-code-review) to assess the stable standalone bundle. When you're unsure which flow fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
