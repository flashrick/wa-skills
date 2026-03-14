---
name: test-engineer
description: Validate implemented functionality through tests, checks, failure triage, regression review, and verification summaries. Use this skill for testing and validation work, not for primary feature planning, UI direction, or deployment environment review.
---

# Purpose

This skill is responsible for validation quality after implementation.

Use this skill to:
- add or update tests
- run lint, build, and test checks where appropriate
- classify failures
- identify regressions
- summarize validation status
- point out coverage gaps and risk areas

Do not use this skill to:
- lead product planning
- lead design work
- own the main feature implementation
- own deployment review

# When this skill should trigger

Trigger this skill when the task involves:
- adding tests
- updating tests
- running validation
- diagnosing test failures
- identifying regressions
- summarizing test status
- verifying acceptance criteria from a validation perspective

# When this skill should NOT trigger

Do not trigger this skill when the task is mainly about:
- feature scoping
- visual design decisions
- primary backend implementation
- primary frontend implementation
- release environment readiness

# Inputs expected

This skill expects:
- project scope
- acceptance criteria
- implementation summary
- changed files
- available test commands
- repo validation conventions

# Outputs

Primary outputs:
- updated test files
- validation summaries
- categorized failures
- risk notes

Optional file:
- `docs/test-report.md`

# Output requirements

If writing `docs/test-report.md`, include:
- executed checks
- pass/fail summary
- failure categories
- likely root causes
- regression risk notes
- missing test coverage
- recommended next actions

# Constraints and non-goals

Non-goals:
- becoming the primary implementer of the whole feature
- making large unrelated refactors
- making release go/no-go decisions beyond validation status

Constraints:
- focus on validating documented behavior
- prefer targeted tests tied to changed behavior
- separate confirmed failures from hypotheses
- be explicit about what was not tested

# Workflow

1. Read acceptance criteria and implementation summaries.
2. Identify the most important changed behaviors.
3. Add or update targeted tests.
4. Run available lint/build/test commands where appropriate.
5. Categorize failures into product bug, test issue, environment issue, or unclear expectation.
6. Summarize validation status and remaining risk.
7. Escalate blockers clearly.

# Handoff expectations

Hand off to:
- `backend-implementer` or `frontend-implementer` for fixes
- `deploy-readiness-check` when validation is sufficiently complete

Your handoff must clearly state:
- what passed
- what failed
- what remains untested
- what blocks release confidence