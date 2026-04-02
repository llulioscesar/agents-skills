---
name: ship-safe
description: Implement a change and do not consider it complete until it has been tested and reviewed. Use for features, refactors, backend changes, frontend changes, API work, auth changes, or dependency-related work.
disable-model-invocation: true
---

Implement the requested change, validate it, review it, and return a concise final status.

## Workflow

1. Implement the change with the appropriate specialist:
   - For Go changes, use `go-specialist`
   - For other stacks, implement directly — no dedicated specialist exists yet for those stacks

2. Add or update tests when the change affects observable behavior:
   - For Go changes, use `go-specialist`
   - For TypeScript, JavaScript, React, Node.js, or Flutter changes, use `test-writer`

3. Run relevant validation commands when useful:
   - tests
   - typechecks
   - builds
   - targeted validation commands for the changed area

4. Review the code:
   - If Go code changed, use `go-reviewer`
   - If TypeScript, JavaScript, React, Node.js, or Flutter code changed, use `code-reviewer`
   - Both reviewers may run in the same change if multiple stacks are affected

5. Run `security-reviewer` if the change touches any of:
   - backend or API behavior
   - authentication or authorization
   - dependency manifests or lockfiles
   - file upload flows
   - external integrations
   - CI/CD, Docker, infrastructure, environment configuration, secrets handling

6. Return a concise final status.

## Output format

### Delivery summary
- What was implemented

### Tests
- Added or updated tests
- Validation commands run

### Review
- Reviewer findings that still matter after implementation changes
- Security findings if applicable

### Final status
- Ready / Ready with caveats / Needs follow-up

### Remaining risks
- Brief list if anything still needs attention

## Rules

- Do not consider implementation complete until validation and review are done.
- Prefer the smallest coherent change that solves the requested problem.
- Do not add unrelated refactors.
- Add or update tests when behavior changes meaningfully.
- Keep the final output concise and execution-focused.
