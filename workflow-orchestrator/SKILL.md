---
name: workflow-orchestrator
description: Coordinate the order of web-project skills, enforce stage acceptance gates, and decide which skill should act next based on current project state. Use this skill for workflow control and handoff discipline, not for direct requirement interviews, design creation, feature implementation, testing execution, or deployment analysis.
---

# Purpose

This skill controls the workflow across specialized project skills.

Use this skill to:
- determine the next appropriate skill
- enforce stage order
- check whether a stage is complete enough to proceed
- maintain handoff discipline
- prevent overlapping responsibilities
- keep the project moving through explicit phases

Do not use this skill to:
- replace the project manager
- create UI designs directly
- implement backend or frontend features directly
- run the main testing work directly
- perform deployment analysis directly except to assess stage status

# Canonical stage order

Default stage order:
1. project-manager
2. uiux-designer
3. repo-architect
4. backend-implementer
5. frontend-implementer
6. test-engineer
7. deploy-readiness-check

This order can be adjusted only with explicit justification.

# When this skill should trigger

Trigger this skill when the task involves:
- deciding what should happen next
- checking stage readiness
- routing work to the correct skill
- validating handoff completeness
- preventing premature implementation
- coordinating multi-stage execution

# When this skill should NOT trigger

Do not trigger this skill when the task is mainly about:
- gathering initial product requirements directly
- producing the actual UI/UX design deliverables
- writing implementation code
- executing tests as the primary task
- performing deploy-readiness analysis as the primary task

# Inputs expected

This skill expects:
- current project stage
- existing docs in `docs/`
- summaries from previous skills
- known blockers
- acceptance criteria
- changed files if implementation already happened

# Stage acceptance gates

## Gate: project-manager -> uiux-designer
Require:
- scope is documented
- core requirements are clear enough
- technical stack is at least provisionally chosen
- acceptance criteria exist

## Gate: uiux-designer -> repo-architect
Require:
- visual direction is chosen
- page structure is documented
- reusable component direction is documented

## Gate: repo-architect -> backend-implementer / frontend-implementer
Require:
- architecture exists
- API contract is sufficiently defined
- data model direction is sufficiently defined
- implementation order is clear enough

## Gate: backend-implementer -> frontend-implementer
Require:
- backend contract is implemented or mocked clearly enough
- integration assumptions are documented

## Gate: frontend-implementer -> test-engineer
Require:
- target user flow is implemented enough to validate
- known gaps are documented

## Gate: test-engineer -> deploy-readiness-check
Require:
- meaningful validation has been run
- pass/fail status is summarized
- major blockers are categorized

# Outputs

Optional file:
- `docs/workflow-status.md`

If useful, record:
- current stage
- completed stages
- blocked stages
- next recommended skill
- missing documents
- gate status

# Constraints and non-goals

Non-goals:
- doing the content work of specialized skills
- silently changing requirements or architecture
- bypassing missing stage outputs without explanation

Constraints:
- route to the narrowest appropriate skill
- be strict about handoff quality
- keep summaries concise and actionable
- prefer documented decisions over hidden assumptions

# Workflow

1. Inspect the current docs and stage artifacts.
2. Determine current project stage.
3. Check whether the current stage has met its acceptance gate.
4. If not met, identify exactly what is missing.
5. Recommend the next skill explicitly.
6. Record status if useful.
7. Avoid doing the next skill's actual work unless explicitly instructed.

# Handoff expectations

This skill does not produce core implementation artifacts.

Its handoff must clearly state:
- current stage
- next recommended skill
- why that skill is next
- what documents or conditions are missing
- whether the project is blocked, ready, or partially ready