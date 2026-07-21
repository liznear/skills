---
name: critique
description: Run the design-space moves against an existing artifact to expose its hidden assumptions and the design space it chose from.
disable-model-invocation: true
---

# critique

The generative skill `design-space` builds options from a blank page. This skill runs the same moves in reverse — against an artifact that already exists — to expose what its author took for granted and the **adjacent design space** they walked past.

The artifact can be anything someone designed: a paper, a framework, an architecture, a company, a product, a decision, even a narrative's worldbuilding. Whatever it is, someone made bets. This skill finds them.

## The analytical stance

Summarizing repeats what the author said; critique finds what the author *assumed*. Apply each `design-space` move, but pointed back at the artifact instead of forward at a blank page:

| design-space move (generative) | critique move (analytical) |
|---|---|
| Frame the problem | What is the artifact trying to do, and what does it treat as the problem worth solving? |
| Surface hidden assumptions | What does the author assume *without stating*? These are the artifact's **hidden bet**. |
| Reverse each assumption | For each, what design would the flip imply — and does the field offer it? |
| Form design families | What *other* families solve the same problem? Why didn't the author pick them? |
| Locate where complexity lives | Where does *this* artifact hide its complexity? |
| Extreme constraints | Under which extreme constraint does this design break first? |
| Future-change test | How will it age? Which demand shift snaps it? |
| Premortem | If it fails, what's the most likely cause — visible from inside it today? |

## The deliverable

Two things, and only these:

1. **The hidden bet** — the assumptions and constraints the author wagered on without saying so. The artifact is a stance; name the stance.
2. **The adjacent design space** — the families the author could have chosen but didn't, and what would have to be true for one of them to win instead.

This is the analytical form of `design-space`'s *signals to revisit*: instead of "when would I switch," it asks "under the author's own assumptions, which neighbor was one signal away from winning?"

---

For the generative form of these moves — building designs rather than reading them — run `design-space`.
