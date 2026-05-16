---
name: reliability-reviewer
description: Review a scoped service, API, job, or integration for failure semantics, timeouts, retries, load limits, observability, and operational safety before or after implementation.
---

# Reliability Reviewer

## What It Does

- Performs a focused reliability review for a scoped backend path, job, queue, integration, or operational workflow.
- Surfaces failure-mode, timeout, retry, saturation, and observability gaps that ordinary implementation review can miss.
- Produces reliability guidance that implementation, testing, or deploy-readiness work can consume directly.

## When It Should Trigger

- A change touches external dependencies, retries, queues, caches, jobs, background work, or other failure-prone runtime paths.
- Production resilience matters more than the happy path alone.
- The team needs a targeted reliability pass before or after implementation.

## When It Should Not Trigger

- The main task is a final release-go/no-go check; use `deploy-readiness-check`.
- The work is mainly product scoping, repository architecture, or direct implementation.
- The code path has no meaningful runtime, dependency, or operational failure surface.

## Expected Inputs

- Scoped changed behavior or planned design
- Existing implementation summary, if code already exists
- Known dependencies, queues, caches, jobs, or operational assumptions
- Current validation or incident context when available

## Expected Outputs

- `docs/reliability-review.md`
- Reliability findings tied to the scoped path
- Explicit failure-mode and boundary recommendations
- Verification targets for `test-engineer`
- Handoff notes for implementation skills or `deploy-readiness-check`

## Workflow

### 1. Frame The Runtime Path

- Restate the service, API, job, or integration being reviewed.
- Identify dependencies, waits, queues, state transitions, and operational controls that matter.
- Separate confirmed behavior from assumptions immediately.

### 2. Inspect Failure Semantics

- Check timeouts, retries, fallback behavior, validation of dependency responses, and partial-failure handling.
- Look for hidden infinite waits, unbounded retries, silent failure, or caller-survival gaps.
- Keep the review scoped to concrete runtime behavior rather than generic best-practice prose.

### 3. Inspect Demand And Isolation

- Review load limits, queue or pool bounds, bulkheads, slow-work isolation, and back-pressure assumptions.
- Note where one failure can spread or saturate shared resources.
- Call out missing explicit behavior for overload or degraded states.

### 4. Inspect Diagnosis And Operations

- Check observability, health visibility, admin controls, idempotency, restartability, and recovery assumptions.
- Make missing monitoring, auditability, or operational stop paths explicit.
- Distinguish design-level resilience gaps from release-readiness paperwork gaps.

### 5. Prepare Handoff

- Route code changes back to `backend-implementer`.
- Route reliability-focused coverage to `test-engineer`.
- Route final pre-deploy gating to `deploy-readiness-check` once resilience blockers are understood.

## Output Shape

When producing a reliability artifact, prefer this structure:

```md
# Reliability Review

## Scope
- Service, API, job, or integration reviewed

## Failure Semantics
- Timeout, retry, fallback, and validation findings

## Capacity And Isolation
- Queue, pool, saturation, and blast-radius findings

## Observability And Operations
- Health, logging, controls, restartability, recovery

## Recommended Changes
- Highest-value fixes or constraints

## Verification Targets
- Tests or checks needed

## Handoff Notes
- Implementation, testing, or deploy-review follow-up
```

## Handoff Expectations

### To `backend-implementer`

Provide:

- the missing or unsafe runtime behavior
- the specific dependency, queue, timeout, or retry boundary involved
- the smallest practical resilience improvement direction

Do not provide:

- a generic demand to "make it production ready"
- deploy-only checklist items disguised as code changes

### To `test-engineer`

Provide:

- the failure modes, saturation cases, or degraded states worth validating
- any idempotency, timeout, retry, or back-pressure behavior that must be observed explicitly

### To `deploy-readiness-check`

Provide:

- which reliability concerns are already addressed
- which remain blockers or known risks for release confidence

## Non-Goals

- Final release approval
- Broad infrastructure architecture
- Writing the production implementation directly
- Security-only review unless it directly affects operational safety
