---
name: ci-failure-triager
description: Triage failing CI checks, GitHub Actions jobs, build pipelines, or remote test runs by collecting logs, identifying root cause, separating infrastructure from product failures, and routing the fix. Use when CI fails or PR checks are red, not for ordinary local test writing or deploy readiness review.
---

# CI Failure Triager

## What It Does

- Investigates failing CI or remote check results.
- Collects the smallest useful failure logs and command context.
- Classifies failures as code regression, test issue, dependency/environment issue, flaky behavior, workflow configuration, or external service problem.
- Produces a fix route and recheck plan.

## When It Should Trigger

- The user asks to fix or diagnose failing CI, red PR checks, GitHub Actions failures, or build pipeline errors.
- A local command passes but CI fails.
- CI logs need to be summarized before deciding who should fix the issue.
- A failure might be caused by runner environment, caching, missing secrets, version drift, or workflow config.

## When It Should Not Trigger

- The user only asks to add or update tests; use `test-engineer`.
- The failure is reproduced locally and needs code diagnosis; use `bug-investigator` or the relevant implementer.
- The task is release readiness after validation; use `deploy-readiness-check`.
- The task is to address human review comments; use a GitHub PR comment workflow if available.

## Expected Inputs

- CI provider, check name, PR number, branch, or failing command.
- Logs or a link to logs when available.
- Local test/build results, if already run.
- Recent related changes.
- Available CLI tools such as `gh`, provider CLI, or local scripts.

## Expected Outputs

- Failing check summary with command, job, and relevant log excerpt.
- Failure classification and confidence.
- Root cause or next diagnostic step.
- Fix route to `backend-implementer`, `frontend-implementer`, `test-engineer`, config owner, or user setup.
- Recheck command or CI action needed after the fix.

## Preflight

- Check whether required CLIs are installed before relying on them.
- For GitHub Actions, prefer `gh` when available and authenticated.
- If `gh` is missing or unauthenticated, do not silently pretend CI was inspected. Ask for logs or give exact setup steps:
  - install GitHub CLI
  - run `gh auth login`
  - ensure repository and workflow scopes are available
- If logs require network, credentials, or elevated access, stop and request the required access instead of using guesses.

## Workflow

### 1. Locate The Failing Check

- Identify the provider, workflow, job, branch, commit, and failing command.
- If multiple checks fail, start with the earliest root failure, not downstream cancellations.
- For external checks, capture the URL and available status even when logs are unavailable.

### 2. Extract High-Signal Logs

- Pull or read only the relevant failure section.
- Include enough surrounding context to identify the command, environment, and first error.
- Avoid pasting huge logs into the final output; summarize and cite the file or command source.

### 3. Classify The Failure

Classify as one of:

- code regression
- test expectation issue
- missing dependency or install step
- runtime or version mismatch
- missing secret or environment variable
- flaky timing or ordering failure
- workflow or cache configuration issue
- external service failure
- unknown, needs more evidence

### 4. Compare Local And CI Behavior

- Run the closest local command when feasible.
- Compare versions, environment variables, working directory, service dependencies, and test selection.
- Do not assume local pass means CI is wrong; identify the difference.

### 5. Route The Fix

- Send code defects to `backend-implementer` or `frontend-implementer`.
- Send missing or incorrect tests to `test-engineer`.
- Send unclear runtime bugs to `bug-investigator`.
- Send release-blocking configuration or migration readiness concerns to `deploy-readiness-check`.

## Output Shape

```md
# CI Failure Triage

## Failing Check
- Provider, workflow, job, command, and link if available

## Log Signal
- Small relevant excerpt or summary

## Classification
- Category and confidence

## Root Cause Or Next Check
- Evidence-backed cause or diagnostic step

## Fix Route
- Owner skill and likely files

## Recheck
- Command or CI action to confirm
```

## Constraints

- Do not implement speculative fixes without identifying the failure class.
- Do not mark CI as flaky without evidence from retries, timing, or nondeterministic failure signatures.
- Do not request broad credentials when a provided log is enough.
- Do not broaden into deployment readiness unless the failure is release-environment specific.
