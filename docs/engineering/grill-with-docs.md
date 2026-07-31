Quickstart:

```bash
npx skills add mattpocock/skills --skill=grill-with-docs
```

```bash
npx skills update grill-with-docs
```

[Source](https://github.com/mattpocock/skills/tree/main/skills/engineering/grill-with-docs)

## What it does

`grill-with-docs` interviews you relentlessly about a plan or design, one question at a time, until you and the agent reach a shared understanding — and it captures the routed vocabulary and decisions in the active planning ticket as you go.

The grilling **leaves a paper trail**. A plain interview sharpens your thinking and then evaporates when the session ends; this one captures each resolved term and hard, one-way decision in the ticket's single Workflow-Identity-owned Resolution draft, including its canonical owner and later documentation obligation. Planning does not publish that draft into `CONTEXT.md` or `docs/adr/`; an authorized Repository Delivery does that later. The alignment survives the conversation without turning planning into canonical current truth.

## When to reach for it

You invoke this by typing `/grill-with-docs` — the agent won't reach for it on its own.

Reach for it at the very start of a change, when the plan is still fuzzy and the domain language isn't settled, and you want to stress-test both before any code exists. If you only want the interview and don't need the artifacts, use [grilling](https://aihero.dev/skills-grilling); if the plan is already clear and you just need to pin down or record terminology, use [domain-modeling](https://aihero.dev/skills-domain-modeling). And if the change is too big to hold in one session and its route is still foggy — a greenfield project, a huge feature build — start upstream with [wayfinder](https://aihero.dev/skills-wayfinder): it charts the effort as a map of decisions, then hands back to this main flow once the way is clear.

## Prerequisites

This skill is stateful through the configured Work Tracker. It needs an active planning ticket and the repository's Domain Orientation so `/domain-modeling` can identify each canonical owner. The session updates its own Resolution draft as terms and decisions crystallise. If a routed owner is unavailable, it records that condition and the proposed change for later routing instead of creating a local glossary or ADR substitute.

## The grill

The engine is a **grill**: a relentless, one-question-at-a-time walk down the design tree, resolving dependencies between decisions before moving on, with a recommended answer offered for every question. Questions the codebase can answer are answered by reading the codebase, not by asking you.

What makes this variant its own skill is where the answers go. As the grill runs, fuzzy language gets sharpened into proposed canonical terms and captured in the Resolution draft inline — not batched at the end. The draft keeps exact language separate from implementation detail and preserves where the later canonical documentation change belongs. ADRs are offered sparingly, only when a decision is hard to reverse, surprising without context, and the result of a real trade-off. Most sessions produce a sharper domain delta and few or no proposed ADRs, and that's the intended shape.

## It's working if

- It asks one question at a time and waits, rather than dumping a questionnaire.
- Terms and their canonical routing get captured in the owned Resolution draft the moment they resolve, in your project's own words.
- It reaches into the codebase to answer its own questions where it can.
- ADRs stay rare — you're not asked to rubber-stamp reversible choices.

## Where it fits

`grill-with-docs` is the opening step of the main build chain:

```txt
grill-with-docs → to-spec → to-tickets → implement → code-review
```

It comes first, before anything is written down as a spec: it produces the shared understanding and settled, routed domain delta that [to-spec](https://aihero.dev/skills-to-spec) then synthesises without re-interviewing you. Its close neighbours are [grilling](https://aihero.dev/skills-grilling), the same interview without the planning record, and [domain-modeling](https://aihero.dev/skills-domain-modeling), the glossary-and-ADR discipline it drives. When you're unsure which skill or flow fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
