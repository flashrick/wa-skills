---
name: deploy-readiness-check
description: Review a web project for pre-deploy readiness across configuration, migrations, observability, rollback, and release-risk gaps.
---

# Deploy Readiness Check

## What It Does

- Reviews whether a web project is ready for a deployment decision.
- Checks configuration, migrations, operational assumptions, rollback posture, and release-risk gaps.
- Produces a practical readiness summary that makes blockers and unknowns explicit.

## When It Should Trigger

- The project has reached a release candidate state and needs a pre-deploy readiness review.
- The main task is to assess deployment blockers, operational gaps, or launch readiness.

## When It Should Not Trigger

- The task is mainly about feature scoping, design work, primary implementation, or core testing execution.
- The real blocker is still missing implementation or validation rather than deploy review.
- The request is for infrastructure design rather than readiness assessment.

## Expected Inputs

- Implementation summary and changed behavior
- Test or verification summary
- Deployment target assumptions
- Runtime configuration and environment expectations
- Migration, observability, and rollback context if available

## Expected Outputs

- A deployment readiness summary with ready items, blockers, and unknowns
- A concrete checklist for pre-deploy actions and follow-up
- Release-risk notes tied to specific gaps
- Explicit handoff notes for the skill or owner that must unblock release

## Workflow

### 1. Frame The Release Target

- Restate what is intended to be deployed and to which environment.
- Identify the release-critical surfaces: runtime config, data changes, external services, logging, alerting, rollback, and operational dependencies.
- Separate confirmed readiness from assumptions immediately.

### 2. Inspect Release Inputs

- Read implementation and validation summaries first.
- Inspect deployment-related files and docs if they exist.
- Reuse the repo's actual deployment model instead of inventing a new one.

### 3. Check Readiness Areas

- Review configuration completeness, secrets handling, and runtime assumptions.
- Review migrations, seed data, and backward-compatibility risks.
- Review build, startup, health visibility, and observability expectations.
- Review rollback posture and release-blocking unknowns.

### 4. Classify Findings

- Mark each item as ready, blocked, or unknown.
- Tie blockers to a specific missing condition or action.
- Distinguish real release risk from speculative concern.

### 5. Summarize Decision Support

- Produce a checklist the team can execute.
- Make the minimum next steps explicit.
- Do not hide unknowns behind a generic "looks fine" summary.

### 6. Prepare Handoff

- Route code or config blockers back to the relevant implementation skill.
- Route unresolved validation gaps back to `test-engineer`.
- Route cross-skill sequencing issues to `workflow-orchestrator` when coordination is the real problem.

## Questioning Strategy

- Ask questions only when they change release confidence or reveal a deployment blocker.
- Prefer inspecting actual config and release artifacts before asking the user.
- Keep questions operational and evidence-seeking, not speculative.
- Do not broaden into infrastructure architecture design unless the user explicitly redirects the task.

Use questions like these when needed:

- "What environment is this deployment targeting first?"
- "Are there required secrets, migrations, or external services not represented in the repo?"
- "What rollback option exists if this release fails?"
- "What monitoring or logs will confirm the deployment is healthy?"
- "Are any known test failures being accepted for this release?"

Avoid questions like these unless the user explicitly asks for that depth:

- feature-scope questions owned by `project-manager`
- UI design questions owned by `uiux-designer`
- primary implementation design questions owned by implementation skills

## Output Shape

When producing a readiness artifact, prefer this structure:

```md
# Deploy Readiness Summary

## Release Target
- What is being deployed
- Target environment

## Ready
- Confirmed-ready items

## Blocked
- Release blockers
- Required actions

## Unknown
- Missing information that affects confidence

## Checklist
- Pre-deploy actions
- Post-deploy verification actions
- Rollback considerations

## Handoff Notes
- Which skill or owner must act next
```

Keep the summary concrete:

- tie every blocker to a real condition or missing artifact
- separate unknowns from confirmed failures
- focus on release decisions, not generic platform theory

## Handoff Expectations

### To `backend-implementer` or `frontend-implementer`

Provide:

- code or configuration blockers
- exact runtime or build assumptions that failed readiness review
- migration or compatibility concerns
- what must change before deploy confidence improves

Do not provide:

- vague "deployment issue" summaries
- infrastructure redesign requests that are outside readiness review

### To `test-engineer`

Provide:

- validation gaps that block release confidence
- unverified critical paths
- failures that need clearer triage before deployment

Do not provide:

- missing operational information disguised as test work

### To `workflow-orchestrator`

Provide only when coordination is the blocker:

- current readiness status
- which prerequisite stage is incomplete
- what output is missing before deploy review can conclude

## Non-Goals

- Defining product scope or UX direction
- Writing core feature code
- Replacing targeted testing work
- Designing a full infrastructure architecture
- Making unsupported go/no-go claims without evidence
