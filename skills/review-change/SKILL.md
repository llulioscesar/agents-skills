---
name: review-change
description: Review the current change before considering it complete. Use after writing or modifying Go, TypeScript, JavaScript, React, Node.js, Flutter, auth, dependencies, CI/CD, config, uploads, or infrastructure-related code.
disable-model-invocation: true
---

Review the current change with the appropriate reviewers and return a concise final assessment.

## Workflow

1. Inspect which files changed and group them by concern:
   - Go
   - TypeScript / JavaScript
   - React / frontend
   - Node.js backend
   - Flutter
   - auth / authorization
   - dependencies / lockfiles
   - CI/CD / Docker / infra / config
   - uploads / external integrations

2. Run the appropriate code reviewer(s):
   - If Go code changed, use `go-reviewer`
   - If TypeScript, JavaScript, React, Node.js, or Flutter code changed, use `code-reviewer`
   - Both reviewers may run in the same change if multiple stacks are affected

3. Run `security-reviewer` if the change touches any of:
   - backend or API behavior
   - authentication or authorization
   - dependency manifests or lockfiles
   - file upload flows
   - external integrations
   - CI/CD, Docker, infrastructure, environment configuration, secrets handling

4. Merge the findings into one final review:
   - remove duplicates
   - prioritize by severity
   - keep concrete file references when available

## Output format

### Review summary
- Overall status: pass / pass with issues / needs fixes

### Critical
- ...

### Important
- ...

### Minor
- ...

### Security
- ...

### Final call
- Brief recommendation on whether the change is ready or what must be fixed first

## Rules

- Do not re-review unrelated old code unless it directly affects the changed code.
- Prefer concrete findings over generic advice.
- If no meaningful issues are found, say so clearly.
- Keep the final output concise and actionable.
