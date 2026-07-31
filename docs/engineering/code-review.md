Quickstart:

```bash
npx skills add mattpocock/skills --skill=code-review
```

```bash
npx skills update code-review
```

[Source](https://github.com/mattpocock/skills/tree/main/skills/engineering/code-review)

## What it does

`code-review` reviews one or more fixed Repository Targets. It can run the **Standards** axis, the **Spec** axis, or both: Standards produces one repository-local result per target, while Spec produces one result for the complete Delivery Bundle.

Every result stays bound to exact review state. When an influencing head or WIP snapshot changes, its repository validation and Standards result become stale, along with any bundle Spec result that used it.

## When to reach for it

Type `/code-review`, or the agent reaches for it automatically when you ask to review a branch, worktree, PR, Repository Delivery, or complete Delivery Bundle.

Reach for it when you have one or more known-good fixed points and exact heads to judge. Select Standards only for repository-local maintainability loops, Spec only for a stable whole-bundle acceptance pass, or both for the standalone default. For writing the behavior test-first, use [tdd](https://aihero.dev/skills-tdd); for building a whole assignment, use [implement](https://aihero.dev/skills-implement).

## Prerequisites

An explicit multi-repository target records a Repository ID, repository path, fixed point, exact review head, authoritative sources, and relevant settled seams. The single-repository shorthand still accepts the familiar current repository plus fixed point; the skill can discover its sources and seam context, asking only when the selected Spec axis has no authority. Supply repository validation evidence when acceptance depends on it.

An explicit WIP target also records an immutable Git tree object ID captured through a temporary index. If that snapshot changes without changing `HEAD`, the review is still stale. When the single-repository shorthand encounters a dirty worktree without an explicit mode, it asks whether those changes belong to the review before selecting the committed or WIP target.

## Repository Standards, bundle Spec

The two axes stay independent. Standards uses one fresh reviewer per selected repository with that repository's instructions, standards, and Fowler smell baseline. Spec uses one fresh Bundle Spec Reviewer with every selected diff, authoritative source, settled seam, and relevant validation result. Findings remain separated so success on one axis cannot mask failure on the other.

Review is read-only. The skill reports findings and evidence; it does not publish branches, create or update Review Proposals, or change Work Tracker state.

## It's working if

- Every fixed target resolves before reviewers start and has a non-empty selected change set.
- Standards results are separated by Repository ID.
- The Spec result covers the complete selected bundle and routes findings to affected repositories.
- A changed head forces fresh validation and Standards evidence before a current bundle Spec pass.
- Selecting one axis does not silently run the other.

## Where it fits

`code-review` is the review step at the tail of the main build chain:

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

It is also independently useful for a single branch or a multi-repository Delivery Bundle. [implement](https://aihero.dev/skills-implement) calls it after standalone implementation; [ask-matt](https://aihero.dev/skills-ask-matt) helps choose the wider flow.
