---
name: to-tickets
description: Break a plan, spec, or the current conversation into a set of tracer-bullet tickets, each declaring its blocking edges, published to the configured tracker — edges as text in a local file, or native blocking links on a real tracker.
disable-model-invocation: true
---

# To Tickets

Break a plan, spec, or conversation into a set of **tickets** — tracer-bullet vertical slices, each declaring the tickets that **block** it.

Read the shared [federated planning authority contract](../FEDERATED-AUTHORITY.md) first. Use the configured Work Tracker, Routing Label mapping, and Domain Orientation when their bindings exist; preserve standalone Local Markdown behavior when they do not.

## Process

### 1. Gather context

Work from whatever is already in the conversation context. If the user passes a reference (a spec path, Work Tracker item, or URL) as an argument, fetch it and read its full body and comments. Accepted planning Resolutions, Domain Model Deltas, and architecture decisions are authoritative source payloads, not background summaries.

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code. Ticket titles and descriptions should use the project's domain glossary vocabulary, and respect ADRs in the area you're touching.

Look for opportunities to prefactor the code to make the implementation easier. "Make the change easy, then make the easy change." Do not turn exploration into new open-ended discovery or use it to introduce authority absent from the source.

### 3. Draft vertical slices

Break the work into **tracer bullet** tickets.

<vertical-slice-rules>

- Each slice cuts a narrow but COMPLETE path through every layer (schema, API, UI, tests) — vertical, NOT a horizontal slice of one layer
- A completed slice is demoable or verifiable on its own
- Each slice is sized to fit in a single fresh context window
- Any prefactoring should be done first

</vertical-slice-rules>

Give each ticket its **blocking edges** — the other tickets that must complete before it can start. A ticket with no blockers can start immediately.

For each proposed slice, derive the complete executable contract from its source:

- narrow Repository References to the minimal complete set;
- give Repository Scope a non-empty writable subset with an outcome, repository-local seams, and repository-owned validation obligations;
- narrow Context Scope and copy every relevant Domain Model Delta in full with provenance and documentation obligations;
- copy every created, changed, or materially relied-on Cross-Repository Seam and its validation obligations from Testing Decisions.

Do not introduce a repository, context, seam, validation approach, or decision absent from the source. If a material contract field is missing or unsettled, return to clarification or planning instead of publishing the ticket.

**Wide refactors are the exception to vertical slicing.** A **wide refactor** is one mechanical change — rename a column, retype a shared symbol — whose **blast radius** fans across the whole codebase, so a single edit breaks thousands of call sites at once and no vertical slice can land green. Don't force it into a tracer bullet; sequence it as **expand–contract**. First expand: add the new form beside the old so nothing breaks. Then migrate the call sites over in batches sized by blast radius (per package, per directory), each batch its own ticket blocked by the expand, keeping CI green batch to batch because the old form still exists. Finally contract: delete the old form once no caller remains, in a ticket blocked by every migrate batch. When even the batches can't stay green alone, keep the sequence but let them share an integration branch that all block a final integrate-and-verify ticket — green is promised only there.

### 4. Quiz the user

Present the proposed breakdown as a numbered list. For each ticket, show:

- **Title**: short descriptive name
- **Blocked by**: which other tickets (if any) must complete first
- **What it delivers**: the end-to-end behaviour this ticket makes work
- **Executable scope**: writable Repository IDs, relevant contexts, and cross-repository seams

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the blocking edges correct — does each ticket only depend on tickets that genuinely gate it?
- Should any tickets be merged or split further?

Iterate until the user approves the breakdown.

### 5. Publish the tickets to the configured tracker

Publish the approved tickets. **How** depends on the tracker `/setup-matt-pocock-skills` configured — the tickets are the same either way, only the shape of the blocking edges changes:

- **Local Markdown** → write one `tickets.md` in the repository root, all tickets in dependency order (blockers first), with the same complete semantic sections as real Work Tracker tickets.
- **Configured Work Tracker** → publish one item per ticket in dependency order (blockers first) so each ticket's blocking edges can reference durable identifiers. Use the configured native blocking/sub-item relationship where available and its documented fallback otherwise. Apply the mapped `ready-for-agent` Routing Label only after re-reading the resulting item and verifying its complete executable contract and state.

Do NOT close or modify any parent issue.

<tickets-file-template>

# Tickets: <short name of the work>

A one-line summary of what these tickets build. Reference the source spec if there is one.

Work the **frontier**: any ticket whose blockers are all done. For a purely linear chain that means top to bottom.

## <Ticket title>

### Parent

<source specification or planning artifact, or `None — <reason>`>

### What to build

<the end-to-end behaviour this ticket makes work, from the user's perspective — not a layer-by-layer implementation list>

### Acceptance criteria

- [ ] Acceptance criterion 1
- [ ] Acceptance criterion 2

### Repository References

| Repository ID | Remote | Base Branch |
| --- | --- | --- |
| `<id>` | `<remote>` | `<branch>` |

### Repository Scope

- `<writable Repository ID>`
  - Required outcome: <repository-owned result>
  - Repository-local settled seams: <complete applicable seam records, or `None — <reason>`>
  - Repository-owned validation obligations: <commands, methods, and evidence>

### Context Scope

- `<Repository ID>:<repo-relative canonical context path>`
- <complete relevant Domain Model Deltas with provenance and documentation obligations>

### Cross-Repository Seams

<complete applicable seam records and validation obligations, or `None — <reason>`>

### Blocked by

<titles of blocking tickets, or `None — can start immediately`>

## <Ticket title>

...

</tickets-file-template>

<issue-template>

## Parent

A reference to the parent item on the Work Tracker, or `None — <reason>`.

## What to build

The end-to-end behaviour this ticket makes work, from the user's perspective — not layer-by-layer implementation.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2

## Repository References

| Repository ID | Remote | Base Branch |
| --- | --- | --- |
| `<id>` | `<remote>` | `<branch>` |

## Repository Scope

- `<writable Repository ID>`
  - Required outcome: <repository-owned result>
  - Repository-local settled seams: <complete applicable seam records, or `None — <reason>`>
  - Repository-owned validation obligations: <commands, methods, and evidence>

## Context Scope

- `<Repository ID>:<repo-relative canonical context path>`
- <complete relevant Domain Model Deltas with provenance and documentation obligations>

## Cross-Repository Seams

<complete applicable seam records and validation obligations, or `None — <reason>`>

## Blocked by

- A reference to each blocking ticket, or `None — can start immediately`.

</issue-template>

In either form, avoid implementation file paths or code snippets — they go stale fast. Portable repository descriptors, Repository-qualified Context Pointers, canonical artifact pointers, and settled seam locations are required contract fields, not implementation guidance. If a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it and note briefly that it came from a prototype. Trim to the decision-rich parts — not a working demo, just the important bits.

For configured delivery, work the delivery frontier one ticket at a time with `/coordinate-delivery <ticket reference>`. `/implement` remains independently available for explicitly authorized standalone execution.
