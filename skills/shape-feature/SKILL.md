---
name: "shape-feature"
description: "Turn a vague idea, product request, or business problem into a clear product definition before implementation. Use when the request is still fuzzy and needs problem, user, value, scope, constraints, tradeoffs, acceptance criteria, and delivery shape."
disable-model-invocation: true
---

Shape the feature before any implementation begins.

## Workflow

1. Use `product-owner` to transform the request into a clear product definition.
   - Clarify the real problem
   - Identify the primary user or actor
   - Define the desired outcome
   - Bound scope
   - Surface constraints
   - Surface meaningful product tradeoffs
   - Define acceptance criteria
   - Define delivery shape

2. If the request is still too vague or internally inconsistent:
   - do not move forward as if it were ready
   - return the unresolved ambiguities clearly
   - recommend narrowing the request before architecture or implementation

3. If the request is sufficiently defined:
   - use `gap-reviewer` to validate completeness before moving forward
   - if gaps are found, resolve them before recommending the next step
   - return the product definition as the main deliverable
   - recommend the appropriate next step:
     - `technical-shaper` if the feature needs technical shaping
     - `ship-safe` if the feature is already small, clear, and implementation-ready

## Output format

# Shaped Feature: [Feature / Problem Name]

## Product definition
[Return the structured result from product-owner]

## Readiness
- not ready / ready for technical shaping / ready for implementation

## Recommended next step
- clarify more / resolve gaps / move to technical-shaper / move to ship-safe

## Rules

- Do not jump into architecture or code before the feature is clear enough.
- Do not let vague scope pass as implementation-ready.
- Prefer a smaller and clearer feature over a broad and fuzzy one.
- Keep the result in a form that can be handed directly to `technical-shaper` or `ship-safe`.
