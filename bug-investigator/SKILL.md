---
name: bug-investigator
description: Diagnose a reported bug or failing behavior by reproducing it, narrowing the root cause, and producing an evidence-based fix route before implementation. Use when the cause is unclear, symptoms are reported without a known fix, or failures need investigation across logs, tests, code paths, and recent changes.
---

# Bug Investigator

## What It Does

- Turns a bug report or failing behavior into a concrete root-cause hypothesis backed by evidence.
- Reproduces the issue when feasible.
- Narrows the affected code path, data condition, environment, or recent change.
- Produces a fix route and verification targets for the right downstream skill.

## When It Should Trigger

- The user reports a bug but the cause is not known.
- A test or manual workflow fails and needs diagnosis before code changes.
- Logs, screenshots, stack traces, or user symptoms need to be mapped to local code.
- A regression appears after recent changes and the responsible behavior must be isolated.

## When It Should Not Trigger

- The fix is already obvious and implementation can start directly.
- The task is mainly adding tests; use `test-engineer`.
- The task is mainly reviewing a completed diff; use `code-reviewer`.
- The task is specifically CI infrastructure or workflow failure; use `ci-failure-triager`.
- The task is production observability triage in a specific tool such as Sentry or Datadog unless local code investigation is the main work.

## Expected Inputs

- Bug report, failing test, stack trace, console error, screenshot, or reproduction steps.
- Environment details such as browser, OS, runtime version, feature flag, tenant, or seed data when known.
- Recent changes or suspected files, if available.
- Expected behavior and actual behavior.

## Expected Outputs

- Reproduction status: reproduced, not reproduced, or blocked.
- Evidence gathered: commands, logs, traces, screenshots, or code paths inspected.
- Root cause or ranked hypotheses with confidence.
- Minimal fix route and owner skill.
- Verification targets that prove the bug is fixed and does not regress.

## Workflow

### 1. Frame The Symptom

- Restate expected behavior and actual behavior.
- Identify whether the failure is deterministic, intermittent, environment-specific, or data-specific.
- Separate observed facts from user guesses or prior assumptions.

### 2. Reproduce Or Simulate

- Try the narrowest reproduction path first.
- Use existing tests, scripts, dev servers, fixtures, or browser automation if the repo already supports them.
- If reproduction is blocked by setup, record the missing condition and continue with static investigation only when it still provides value.

### 3. Trace The Path

- Follow the failing path from entry point to state mutation, external call, persistence, rendering, or response.
- Compare expected and actual values at the smallest useful boundary.
- Inspect recent related changes when available, but do not assume the latest change is the cause.

### 4. Narrow The Cause

- Eliminate unlikely causes with evidence.
- Prefer one strong root cause over many weak guesses.
- If multiple hypotheses remain, rank them by likelihood and name the next check that would distinguish them.

### 5. Prepare The Fix Route

- Identify whether the fix belongs to `backend-implementer`, `frontend-implementer`, configuration, data repair, or tests.
- Describe the smallest code or data area likely to change.
- Call out risks that need `feature-impact-reviewer`, `web-security-reviewer`, or `test-engineer`.

## Output Shape

```md
# Bug Investigation

## Symptom
- Expected
- Actual

## Reproduction
- Status
- Steps or command

## Evidence
- Logs, traces, code paths, or observations

## Root Cause
- Cause or ranked hypotheses

## Fix Route
- Recommended owner and likely files

## Verification
- Checks needed after the fix
```

## Constraints

- Do not make speculative fixes before enough evidence exists.
- Do not hide failed reproduction attempts.
- Do not broaden into unrelated cleanup.
- Do not treat flaky or environment-only behavior as a product bug without evidence.

## Handoff Expectations

### To Implementation Skills

Provide:

- the smallest failing path
- the likely files or functions
- the exact condition that triggers the bug
- expected post-fix behavior

### To `test-engineer`

Provide:

- the reproduction case to encode as a regression test
- any fixture, seed data, viewport, or environment detail required
- whether the bug was deterministic or intermittent

### To `ci-failure-triager`

Route there when:

- the issue only occurs in CI
- the failure appears tied to runner setup, dependency caching, workflow config, or remote check behavior
