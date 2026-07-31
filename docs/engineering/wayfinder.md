Quickstart:

```bash
npx skills add mattpocock/skills --skill=wayfinder
```

```bash
npx skills update wayfinder
```

[Source](https://github.com/mattpocock/skills/tree/main/skills/engineering/wayfinder)

## What it does

`wayfinder` takes an effort too big for one agent session — wrapped in fog, where the way from here to the goal isn't visible yet — and charts it as a **shared map** of decision tickets on the configured Work Tracker. It **plans, it doesn't do**: every ticket resolves a decision, and the map is done when nothing is left to decide before someone builds the thing.

## When to reach for it

You invoke this by typing `/wayfinder` — the agent won't reach for it on its own.

Reach for it when an effort is **more than one agent session can hold** and the route to its **destination** is still foggy — you can feel the shape of the work but can't yet write it down as a spec or a plan. For turning an *already-clear* thread into a spec, use [to-spec](https://aihero.dev/skills-to-spec); for slicing an already-understood plan into buildable tickets, use [to-tickets](https://aihero.dev/skills-to-tickets). Wayfinder sits upstream of both: it's what you run when there's too much fog to spec directly.

## Prerequisites

Wayfinder reads the configured Work Tracker binding and Domain Orientation before charting or resolving. Its Notes carry relevant portable Domain Federation Home identity and Repository-qualified Context Pointers. Without a binding and outside a configured federation, it preserves the standalone Local Markdown fallback.

## The map is an index, fog is the frontier

The **map** is one Work Tracker item carrying `wayfinder:map`; its decision tickets are child items. It is an **index, not a store**: each decision lives in exactly one ticket, and the map only gists and links. A session loads the map at low resolution and zooms into individual tickets on demand.

Beyond the live tickets lies the **fog of war** — decisions you can tell are coming but can't yet pin down. The test for whether something is a ticket or still fog is whether you can *state the question precisely now*, not whether you can answer it. Resolving a ticket clears the fog ahead of it, **graduating** whatever's now specifiable into fresh tickets. The **frontier** is the open, unblocked, unclaimed tickets — the edge of the known — and it's what the tracker's native blocking renders visually, so you see what's takeable without opening the map. Fog only gathers *toward* the **destination**; work past it is ruled **out of scope**, closed, never graduating.

Every ticket is **HITL** (human in the loop — grilling, prototype) or **AFK** (agent alone — research); a HITL ticket only resolves through a live exchange. Parallel research starts only after each ticket is confirmed on the open, unblocked, unassigned Wayfinder frontier and claimed.

## It's working if

- Naming the **destination** is the first act — before any ticket exists — because it fixes the scope every ticket is measured against.
- One map is one `wayfinder:map` Work Tracker item; tickets are its child items, referred to by **name**, never a bare `#42`.
- A session hand-resolves **at most one ticket**; claimed frontier research may be delegated in parallel.
- Each ticket maintains one complete `## Resolution draft`, finalizes that same comment as `## Resolution` (or uses the configured supersession fallback), closes, and adds only a context pointer to *Decisions so far*.
- If the opening grill surfaces **no fog**, it stops and tells you the journey is small enough to skip the map.

## Where it fits

`wayfinder` is a big-idea **on-ramp**: an effort too large and foggy to spec in one sitting generates a cleared map of decisions, which then merges onto the main build flow. When the fog is pushed back and the way is clear, hand off to [to-spec](https://aihero.dev/skills-to-spec) to schedule the multi-session build (or, if the effort turned out small, implement directly). It leans on [grilling](https://aihero.dev/skills-grilling) and [domain-modeling](https://aihero.dev/skills-domain-modeling) to resolve individual tickets, and on [prototype](https://aihero.dev/skills-prototype) and [research](https://aihero.dev/skills-research) for the ticket types that need them. When you're unsure which skill or flow fits, [ask-matt](https://aihero.dev/skills-ask-matt) routes you.
