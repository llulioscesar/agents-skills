---
name: "release-check"
description: "Evaluate whether a change is actually ready to ship after implementation, validation, review, and risk analysis. Use before deployment or sign-off to confirm security, risk, functional behavior, completeness, and rollout readiness."
disable-model-invocation: true
---

Run a final release-readiness check before deployment or sign-off.

## Workflow

1. Review the current state of the change:
   - implementation status
   - validation status
   - review status
   - security status
   - known risks
   - open gaps
   - rollout expectations

2. If code review has not been completed:
   - use `review-change` before proceeding
   - do not treat unreviewed code as release-ready

3. Use `risk-reviewer` to assess whether the rollout and operational posture are safe enough.
   - identify blast-radius concerns
   - identify rollback or reversibility concerns
   - identify rollout constraints
   - identify missing safeguards

4. Use `security-reviewer` if the change touches backend, auth, dependencies, infrastructure, uploads, integrations, secrets, CI/CD, or any security-sensitive behavior.
   - surface any remaining security blockers or caveats

5. Use `qa-functional` if the change includes meaningful user flows, state transitions, permissions, or recovery behavior.
   - confirm that functional behavior is complete enough to trust

6. Use `gap-reviewer` if there is any sign that the change may still be underdefined, partially validated, or not truly release-ready.
   - confirm that no important gaps are being mistaken for completion

7. Consolidate the results into a final release-readiness decision:
   - ready
   - ready with safeguards
   - not ready

## Output format

# Release Check: [Feature / Change Name]

## Release summary
[Short summary of whether this is actually ready to ship]

## Status
- ready / ready with safeguards / not ready

## Key findings
- ...

## Required safeguards
- ...

## Remaining risks
- ...

## Functional concerns
- ...

## Security concerns
- ...

## Open gaps
- ...

## Rollout recommendation
- ...

## Recommended next step
- deploy / deploy with safeguards / resolve blockers / revisit implementation / move to bug-hunt if release readiness is blocked by a defect

## Rules

- Do not treat “code is done” as “release is safe.”
- Do not ignore unresolved risks just because the feature works in the happy path.
- Do not over-block low-risk changes, but do not understate meaningful release concerns.
- Prefer a clear ship / ship-with-safeguards / do-not-ship decision over vague commentary.
- Keep the result focused on release readiness, not on redesigning the whole feature.
