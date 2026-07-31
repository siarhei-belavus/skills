Quickstart:

```bash
npx skills add mattpocock/skills --skill=to-tickets
```

```bash
npx skills update to-tickets
```

[Source](https://github.com/mattpocock/skills/tree/main/skills/engineering/to-tickets)

## What it does

`to-tickets` transforms settled source authority into self-contained **delivery tickets** — each a tracer-bullet vertical slice — and publishes them to the configured Work Tracker or Local Markdown, with every ticket declaring its blocking edges.

Every ticket is a **tracer bullet** — a thin *vertical* slice that cuts through all integration layers end-to-end (schema, API, UI, tests), never a horizontal slice of one layer. A completed slice is demoable or verifiable on its own, which is what makes each ticket safe to hand to an agent.

## When to reach for it

You invoke this by typing `/to-tickets` — the agent won't reach for it on its own.

Reach for it once you have an agreed plan or a written spec and you want it split into tickets. Point it at the conversation, or pass a specification or Work Tracker reference and it fetches the body and comments first. If the change has not been written up yet, produce a specification first with [to-spec](https://aihero.dev/skills-to-spec).

## Prerequisites

`to-tickets` uses the configured Work Tracker, Routing Label mapping, and Domain Orientation when present. On a real tracker it applies `ready-for-agent` only after re-reading and validating the complete executable ticket and resulting state.

## One artifact, two readings

The blocking edges are the whole point. They make one set of tickets read two ways, depending on the tracker:

- **Local Markdown** → one file per ticket under `.scratch/<feature>/issues/`, with the complete executable contract and textual blocking edges.
- **Configured Work Tracker** → one item per ticket, with native blocking links where available and the configured fallback otherwise. Any open, unblocked, unassigned, correctly routed ticket is on the delivery **frontier**.

The edges live in the ticket regardless of medium; the medium only decides whether anything acts on them in parallel. `to-tickets` produces the artifact — how you run it (sequential by hand, or a parallel fleet) is up to you.

## Vertical slices, not horizontal ones

The whole skill turns on one distinction. A **horizontal** slice ships one layer of the change — all the schema, or all the API — and nothing works until every layer lands. A **vertical** slice, the tracer bullet, ships one narrow path through *every* layer at once, so it can be demoed the moment it's done.

Before slicing, `to-tickets` looks for prefactoring — "make the change easy, then make the easy change" — and orders that work first. It then quizzes you on the breakdown (granularity, blocking edges, what to merge or split) before publishing anything, and publishes blockers first so each ticket's "Blocked by" can reference a real ticket.

## The wide-refactor exception

One shape breaks the tracer-bullet rule: a **wide refactor** — a single mechanical change whose **blast radius** means no independently green vertical slice can contain it. `to-tickets` sequences it as a coordinated cutover: mechanical batches sized by blast radius share an integration branch and block one final integrate-and-verify ticket that establishes the single final form and restores green CI. Old and new forms coexist only when the source specification explicitly authorizes external compatibility and its removal condition.

## Every ticket is executable

Real and Local Markdown tickets carry the same public sections: Parent, What to build, Acceptance criteria, Repository References, non-empty writable Repository Scope, Context Scope, Cross-Repository Seams, and Blocked by. Each carries only source-authorized repositories, contexts, decisions, seams, validation obligations, and dependencies; missing material authority returns to planning instead of being invented.

## Where it fits

`to-tickets` is a step in the main build chain:

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

It sits between [to-spec](https://aihero.dev/skills-to-spec), which supplies settled solution authority, and configured `/coordinate-delivery <ticket reference>` execution. [implement](https://aihero.dev/skills-implement) remains available for explicitly authorized standalone work. Work the frontier one ticket per fresh context. When you're unsure which skill or flow fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
