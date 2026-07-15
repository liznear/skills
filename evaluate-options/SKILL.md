---
name: evaluate-options
description: Systematically compare options using a context-driven rubric. Use when the user wants to evaluate options, choose between paths, or compare alternatives.
---

# evaluate-options

## Steps

Follow these steps in order. Complete each step's criterion before moving to the next.

1. **Ground in the Scenario**
   Define the exact requirements, constraints, and context. If the requirements are ambiguous, ask the user targeted questions to crystallize them before proceeding.
   *Completion criterion:* The scenario and its hard constraints are explicitly written out.

2. **Define the Rubric**
   Design independent evaluation criteria tailored specifically to the scenario. Keep dimensions distinct and orthogonal.
   *Completion criterion:* A list of criteria, each defining what "good" means for this specific scenario.

3. **Build the Matrix**
   Evaluate every provided option against every criterion in the rubric. For each intersection, state how the option performs and explain the technical rationale. Render the final comparison as a clear, scannable Markdown table.
   *Completion criterion:* Every option has a justified stance for every criterion in the rubric, and the final synthesis is presented in a Markdown table.

4. **Surface Alternatives**
   Identify at least one alternative the user did not explicitly provide that fits the scenario, and briefly score it against the rubric.
   *Completion criterion:* One or more unprompted alternatives are introduced and evaluated.
