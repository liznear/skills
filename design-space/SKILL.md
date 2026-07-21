---
name: design-space
description: Explore the **design space** before committing to a design, to prevent **premature convergence** on the first plausible option. Use when facing an open-ended design question — "how should I design X?", an RFC, an architecture or API design, a database schema, a workflow, an agent runtime, a product decision, or a large refactoring. This opens the space; once you have a shortlist, hand it to `evaluate-options` to score against a rubric.
---

# design-space

Open the **design space** before committing. The enemy is **premature convergence** — grabbing the first design that fits, before the alternatives that fit better are even visible. This skill makes those alternatives visible.

It is generative: it produces designs from a blank page. Its analytical twin is `critique`, which runs the same moves against an existing artifact (a paper, framework, company, decision). Once this skill yields a shortlist of **design families**, score them with `evaluate-options` — that skill owns the rubric; this one owns everything before it.

## The spine

Run the steps in order — each step's condition must hold before the next begins. Designing in Step 1 is premature convergence; naming a winner in Step 4 is premature convergence. The ordering is the defence.

### 1. Frame the problem

State, in plain fields:

- **Goal** — the one thing that must be true for this to count as done.
- **Hard constraints** — invariants the design cannot violate.
- **Soft constraints** — preferences that bend.
- **Unknowns** — what you don't yet know and may have to assume.

*Done when* every field is named and the goal is a single sentence, with no design yet proposed.

### 2. Surface hidden assumptions

The *obvious* design is the one you'd reach for in the first five minutes; its assumptions are the soil it grows from. List them — unstated premises the obvious design treats as true. Aim for 5–10.

This is the step that earns the whole skill. Shallow assumption-hunting here makes every later step shallow, because the reversals in Step 3 can only flip what Step 2 found.

- A1. State is mutable.
- A2. The database is the source of truth.
- A3. The workflow is predefined.
- A4. Execution is synchronous.
- A5. A central scheduler exists.
- A6. Every task has the same runtime.

*Done when* 5–10 assumptions are written, each a declarative claim the obvious design depends on.

### 3. Reverse each assumption

For every assumption, flip it and ask: *if this were false, what design does the flip make natural?* Each reversal is a seed for a genuinely different design — a new philosophy, not a tweak of the obvious one.

- "Workflow is predefined" → workflow is generated at runtime → **agent-driven execution**.
- "Database is the source of truth" → an append-only event log is → **event sourcing**.
- "State is mutable" → state is immutable → **append-only log**.

*Done when* every assumption from Step 2 has a reversal and a design the reversal opens up.

### 4. Form design families

Collect the reversals into **3–5 design families** — each a distinct *philosophy* about how the problem is solved. Two designs that agree on every assumption are the same family in two costumes; keep the philosophy, drop the costume.

- **Family A — Orchestrator:** central scheduler, state machine, DB.
- **Family B — Event-driven:** event sourcing, choreography.
- **Family C — Agent runtime:** an LLM decides; the runtime only executes.
- **Family D — Compiler:** the workflow compiles to a static DAG.

*Done when* 3–5 families stand, and each pair disagrees on at least one assumption from Step 2.

### 5. Locate where complexity lives

**Complexity never disappears.** Each family relocates it; the only question is *where it hides*. For every family name the hiding-place, the advantage it buys, the failure mode it invites, and the bottleneck it breaks on.

- Orchestrator → complexity hides in **scheduling**.
- Event-driven → complexity hides in **consistency**.
- Compiler → complexity hides in **expressiveness** (what the DAG can't say).

*Done when* each family has its hiding-place, advantage, failure mode, and bottleneck named.

### 6. Apply extreme constraints

Throw in a constraint the obvious design quietly assumed away, then ask: *forced to satisfy this, how does the design change?* New families fall out of constraints that break the obvious one.

- No database. No cache. No network. Fully offline. 100× traffic. One developer. 100 ms latency. Millions of users. Everything must replay. Everything must be deterministic.

*Done when* 2–3 extreme constraints have been applied, and each has surfaced a design move — a new family, or a reshaping of an existing one.

### 7. Run the future-change test

Fast-forward a year. For a few plausible demand shifts, ask which family *evolves* easily and which *snaps*. New families can appear here too — a shift the current families handle badly is a gap.

- Needs a plugin ecosystem. Needs distributed execution. Needs replay. Needs audit. Needs user scripting. Needs AI planning.

*Done when* each family has an evolvability verdict for at least two future demands.

### 8. Premortem

Assume the chosen family ships and the project fails six months later. Name the **top 5 reasons why**, in the family's own terms — its failure mode and bottleneck are the usual suspects.

- Traffic dwarfed the estimate. The workflow went dynamic. The DB became the bottleneck. Coupling made it untestable. Debugging was impossible.

*Done when* five failure causes are written, each tied to a property of a family under consideration.

### 9. Decide, and leave the door open

Name the **recommended family** and *why*; name the **rejected families** and the single strongest reason each lost. Then — the real deliverable — list **signals to revisit**: the future conditions under which you'd switch. A decision is not "A is best"; it is "A, *unless* these signals appear, in which case B."

- Signal: concurrency > 1000 → revisit Family B.
- Signal: replay becomes required → revisit Family C.
- Signal: a plugin ecosystem emerges → revisit Family D.

*Done when* a recommendation, the rejections, and a signals-to-revisit list are all written.

To turn the shortlist into a scored comparison, hand the families to `evaluate-options` — it builds the rubric; this skill does not.

---

## Producing a written document

When the output is a standalone design doc, render it from [TEMPLATE.md](TEMPLATE.md). Load the template only then — the spine above is enough to run the exploration without it.
