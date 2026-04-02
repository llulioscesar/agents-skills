---
name: "build-plan"
description: "Turn a shaped feature into an executable implementation plan before coding begins. Use after `shape-feature` when the product definition is clear and now needs technical shaping, sequencing, and execution planning."
disable-model-invocation: true
---

Build an implementation plan before any significant coding begins.

## Workflow

1. Use `technical-shaper` to transform the shaped feature into a clear technical shape.
   - Identify affected boundaries
   - Identify technical areas involved
   - Identify main modules or components
   - Identify integrations and dependencies
   - Surface technical constraints
   - Surface meaningful technical tradeoffs
   - Define implementation sequence
   - Surface technical risks

2. If the technical shape is still too unclear, underdefined, or non-executable:
   - do not move forward as if it were implementation-ready
   - return the open technical questions clearly
   - recommend clarifying the technical shape before planning execution

3. If the technical shape is sufficiently defined:
   - use `gap-reviewer` to validate completeness of the plan
   - if meaningful gaps are found, resolve them before recommending execution
   - return the implementation plan as the main deliverable

4. If the plan has meaningful rollout or operational risk:
   - use `risk-reviewer` before recommending execution
   - surface required safeguards, rollback expectations, and rollout constraints

5. If the feature involves non-trivial user flows, state transitions, or permissions:
   - use `qa-functional` to validate behavioral completeness before execution
   - if functional gaps are found, resolve them before recommending `ship-safe`

6. Recommend the appropriate next step:
   - `technical-shaper` if more shaping is still needed
   - `risk-reviewer` if delivery or rollout risk is material
   - `qa-functional` if functional behavior still needs review
   - `ship-safe` if the plan is clear enough to execute

## Output format

# Implementation Plan: [Feature / Change Name]

## Technical shape
[Return the structured result from technical-shaper]

## Execution readiness
- not ready / ready with clarifications / ready for execution

## Recommended next step
- clarify technical shape / resolve gaps / review risk / review functional behavior / move to ship-safe

## Rules

- Do not jump into coding before the plan is executable enough.
- Do not treat vague sequencing as a real implementation plan.
- Prefer a smaller, clearer execution path over a broad and fuzzy plan.
- Keep the result in a form that can be handed directly to `ship-safe`.
