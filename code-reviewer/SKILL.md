---
name: code-reviewer
description: Review a scoped code change, branch, pull request, or patch for correctness, maintainability, contract risk, error handling, and missing validation. Use this skill for engineering code review after or during implementation, not for security-only review, regression-impact-only review, primary implementation, or test execution.
---

# Code Reviewer

## What It Does

- Reviews a bounded code change for defects and engineering risk.
- Prioritizes findings by severity with file and line references when available.
- Checks correctness, maintainability, API contracts, data handling, error paths, concurrency or lifecycle risk, and missing validation.
- Produces a concise review that helps the implementer fix the highest-value issues first.

## When It Should Trigger

- The user asks for a code review, PR review, patch review, or change review.
- A feature implementation is complete enough to inspect for bugs before testing or release.
- A risky change needs an engineering quality pass that is broader than one regression question.
- The user asks whether a diff is safe, maintainable, or likely to break contracts.

## When It Should Not Trigger

- The task is primarily product scoping, UI design, architecture planning, implementation, test writing, or deployment readiness.
- The user only wants security review; use `web-security-reviewer`.
- The user only asks what existing behavior could regress; use `feature-impact-reviewer`.
- The user needs test execution or coverage changes; use `test-engineer`.
- There is no code, patch, branch, or concrete changed area to review.

## Expected Inputs

- Changed files, diff, branch, PR, or implementation summary.
- Acceptance criteria or intended behavior, if available.
- Relevant tests, contracts, schemas, or docs.
- Existing review comments, if this is a follow-up review.

## Expected Outputs

- Findings first, ordered by severity.
- File and line references for each actionable issue when available.
- Open questions or assumptions that affect confidence.
- Brief test or verification gaps when they materially affect the review.
- Handoff notes for `backend-implementer`, `frontend-implementer`, `test-engineer`, `feature-impact-reviewer`, or `web-security-reviewer` when follow-up belongs elsewhere.

## Workflow

### 1. Establish Review Scope

- Identify the exact diff, files, branch, or PR under review.
- Read the repository instructions and nearby code before forming findings.
- Use the stated acceptance criteria when available; otherwise infer intended behavior from the change and surrounding tests.
- Avoid broad repository auditing unless the user explicitly asks for it.

### 2. Inspect Behavior And Contracts

- Check whether the implementation satisfies the intended behavior.
- Look for broken API contracts, schema drift, serialization changes, permission assumptions, lifecycle ordering, state synchronization, and backward compatibility issues.
- Trace shared helpers, public interfaces, and call sites only as far as needed to validate risk.

### 3. Inspect Failure Paths

- Review validation, error handling, retries, cleanup, null or empty states, boundary values, and partial-failure behavior.
- For async or stateful code, check ordering, cancellation, race conditions, stale state, and idempotency.
- For data changes, check migration assumptions, default values, uniqueness, and compatibility with existing rows or clients.

### 4. Inspect Maintainability

- Prefer concrete maintainability issues that can become bugs or slow future changes.
- Check duplication only when it creates real drift risk.
- Check naming or structure only when it obscures behavior, contracts, or ownership.
- Avoid style-only comments unless they hide a correctness problem.

### 5. Inspect Validation Gaps

- Identify missing tests only when the gap could hide a material defect.
- Tie each test gap to a behavior, edge case, contract, or regression risk.
- Do not turn the review into full test planning; hand off to `test-engineer` when coverage work is needed.

## Output Shape

Use this structure for review output:

```md
# Code Review

## Findings
- Severity: file:line - Issue and impact.

## Open Questions
- Question or assumption that affects confidence.

## Test Gaps
- Missing validation tied to a concrete risk.

## Handoff Notes
- Who should act next and why.
```

If there are no findings, say that clearly and still state residual risk or unverified areas.

## Severity Guidance

- Critical: likely data loss, auth bypass, severe outage, or unsafe destructive behavior.
- High: likely user-visible bug, broken contract, migration failure, or major regression.
- Medium: edge-case bug, unclear failure path, or maintainability issue with credible future risk.
- Low: minor correctness or clarity issue that is worth fixing but unlikely to block.

## Handoff Expectations

### To Implementation Skills

Provide:

- the exact failing behavior or risky code path
- why the current change is unsafe or incomplete
- the smallest likely fix direction when obvious

Do not provide:

- broad rewrites without a concrete defect
- style preferences disguised as blockers

### To `test-engineer`

Provide:

- specific behaviors or edge cases that need automated coverage
- the existing tests most likely to be extended, if known
- any environment or fixture assumptions that affect validation

### To Specialist Reviewers

Route to:

- `web-security-reviewer` for auth, permission, sensitive-data, injection, browser-security, or trust-boundary issues
- `feature-impact-reviewer` for focused regression mapping across existing behavior
- `deploy-readiness-check` for release configuration, migration rollout, rollback, or observability readiness

## Non-Goals

- Writing the implementation
- Running the full test suite
- Acting as a security-only reviewer
- Performing full-repository architecture audit
- Replacing acceptance criteria, design, or product scope
