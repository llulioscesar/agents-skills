---
name: bug-hunt
description: Investigate and fix a bug with root-cause discipline. Use for regressions, crashes, broken UI state, unexpected runtime behavior, failing commands, or incorrect output.
disable-model-invocation: true
---

Investigate the bug systematically, identify the root cause, apply the smallest safe fix when confirmed, and add a regression test when appropriate.

## Workflow

1. Use `Explore` to locate the relevant code paths, related files, configs, and nearby tests.
   - Focus on the code paths most likely related to the reported symptom.
   - Include surrounding context only as needed.

2. Use `debugger` to investigate:
   - summarize the symptom
   - form ranked hypotheses
   - inspect evidence
   - validate the root cause
   - apply the smallest safe fix if the root cause is confirmed

3. Add or update tests when the fix changes observable behavior:
   - Only proceed if the root cause was confirmed and the fix was applied.
   - For Go changes, use `go-specialist`
   - For TypeScript, JavaScript, React, Node.js, or Flutter changes, use `test-writer`
   - If the root cause could not be confirmed, skip this step and surface the open question in the final report.

4. Run relevant validation commands when useful:
   - tests
   - typechecks
   - builds
   - targeted commands that validate the fix

5. Return one concise final report.

## Output format

### Bug summary
- What was broken
- What should have happened

### Root cause
- Clear explanation of why the bug happened

### Fix
- What changed
- Why this fix addresses the root cause

### Validation
- What was checked

### Regression prevention
- Test added or updated
- Related area to watch if relevant

## Rules

- Do not jump straight to patching before the root cause is explained.
- Prefer the smallest fix with the lowest blast radius.
- Do not refactor unrelated code during a bug fix.
- Add a regression test when the bug is meaningfully testable.
- Keep the final output concise and concrete.
