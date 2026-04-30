---
name: workflow-orchestrator
description: Coordinate multi-skill web-project execution by assigning stage order, handoffs, and parallel work boundaries across specialized skills.
---

# Workflow Orchestrator

## What It Does

- Coordinates the order of specialized web-project skills.
- Checks whether handoffs are complete enough for the next stage.
- Recommends the next skill and any safe parallel work boundaries without doing that skill's content work.

## When It Should Trigger

- The main task is to decide what skill should act next, whether a stage is ready, or how multi-skill work should be sequenced.
- The project has multiple possible next steps and needs disciplined handoff control.

## When It Should Not Trigger

- The task is mainly about requirement clarification, design creation, architecture design, implementation, testing, or deploy assessment.
- The next skill is already obvious and no gating or sequencing decision is needed.
- The user wants this skill to do the actual work of a specialized skill.

## Expected Inputs

- Current project stage or current state if known
- Existing docs, handoff notes, or summaries from prior skills
- Known blockers and unresolved assumptions
- Acceptance criteria or release goal when available
- Relevant changed files or implementation summaries if work has already started

## Expected Outputs

- A current-stage assessment
- A next-skill recommendation with reasoning
- Explicit gate checks showing what is ready, blocked, or missing
- Safe parallel work boundaries when parallel execution is justified

## Workflow

### 1. Identify The Current Stage

- Restate the current project state in terms of actual artifacts, not guesses.
- Determine whether the work is in scoping, design, architecture, implementation, validation, or release review.
- If the stage is ambiguous, use the available outputs to infer the farthest completed stage conservatively.

### 2. Check Handoff Completeness

- Inspect the outputs from the current or previous stage.
- Verify whether the next stage has enough information to proceed safely.
- Treat missing artifacts, missing acceptance criteria, or unresolved blockers as gate failures.

### 3. Recommend The Next Skill

- Route to the narrowest skill that solves the actual blocker.
- Explain why that skill is next and why other skills should wait.
- Only recommend parallel work when the write scopes or decision scopes are genuinely independent.

### 4. Make Missing Conditions Explicit

- List what is blocking progress, who owns it, and what artifact or decision is missing.
- Distinguish a true blocker from a nice-to-have.
- Do not bypass a missing handoff by inventing the downstream work.

### 5. Record Status

- Produce a concise workflow status summary when useful.
- Keep it actionable enough that the next skill can start immediately.
- Avoid turning the status into a generic project-management report.

## Questioning Strategy

- Ask questions only when they change stage routing or gate decisions.
- Prefer reading existing artifacts before asking the user for status.
- Keep questions about missing outputs, blockers, or sequencing constraints.
- Do not broaden into product discovery, design review, or technical implementation debate.

Use questions like these when needed:

- "What is the latest completed artifact or stage for this work?"
- "Is the current blocker missing scope, missing design, missing architecture, missing implementation, or missing validation?"
- "Are any teams or files safe to move in parallel, or is there a shared dependency?"
- "Which unresolved decision is preventing the next skill from starting cleanly?"
- "Is there a release target that changes the recommended order?"

Avoid questions like these unless the user explicitly asks for that depth:

- scope-definition questions owned by `project-manager`
- UX questions owned by `uiux-designer`
- architecture or implementation questions owned by the corresponding specialist skill

## Output Shape

When producing a workflow status artifact, prefer this structure:

```md
# Workflow Status

## Current Stage
- Current stage
- Evidence for that stage

## Gate Check
- Ready items
- Missing items
- Blockers

## Next Skill
- Recommended skill
- Why it is next

## Parallel Work
- Safe parallel tracks
- Shared dependencies

## Handoff Notes
- What the next skill needs
- What must be resolved first
```

Keep the status concrete:

- route based on actual artifacts, not generic phase theory
- name the exact missing handoff or document
- avoid doing the downstream skill's substantive work

## Gate Expectations

### Immediate Coding Plan -> `task-plan-generator`

Require:

- concrete task prompt
- known scope or safe assumptions
- enough repository context to identify likely validation

### Unfamiliar Code Area -> `codebase-recon`

Require:

- target repo, module, feature, bug area, or planned change
- reason orientation is needed before implementation or review

### Unknown Bug -> `bug-investigator`

Require:

- reported symptom or failing behavior
- expected behavior if known
- available logs, screenshots, failing tests, or reproduction notes

### Broad Migration -> `codebase-migration-planner`

Require:

- target migration, upgrade, rename, or refactor goal
- reason the change spans multiple files or modules
- known compatibility constraints or validation expectations

### `project-manager` -> `uiux-designer`

Require:

- scoped objective
- in-scope and out-of-scope boundaries
- acceptance criteria or equivalent success definition

### `uiux-designer` -> `repo-architect`

Require:

- defined user flows or screen intent
- required states and interaction expectations
- design-system brief when reusable UI rules are needed
- feature-specific UI quality gates when the feature is UI-heavy
- constraints that affect structure or implementation

### `repo-architect` -> `backend-implementer` / `frontend-implementer`

Require:

- clear module or ownership boundaries
- known interface seams
- dependency-aware implementation order

### Implementation -> `test-engineer`

Require:

- implemented behavior to validate
- changed-area summary
- UI quality gates that still need evidence, when relevant
- browser-risk notes for overlays, responsive layout, focus, animation, charts, media, or visual assets
- known limitations or unverified areas

### Implementation -> `code-reviewer`

Require:

- concrete diff, branch, PR, or changed files
- intended behavior or acceptance criteria when available
- known risky areas or reviewer focus if relevant

### Failed Local Or CI Checks -> `ci-failure-triager`

Require:

- failing command, CI check, or log source
- branch, PR, commit, or local environment context when available
- whether the failure reproduces locally, if known

### PR Feedback -> `pr-feedback-resolver`

Require:

- pasted comments, PR URL, review thread summary, or issue feedback
- current branch or diff context when available
- user selection if only some comments should be addressed

### `test-engineer` -> `deploy-readiness-check`

Require:

- meaningful validation results
- unresolved failures or risk summary
- enough evidence to assess release confidence

## Optional Skill Routing

These skills are sidecar or persistence skills, not mandatory core stages.

### Route to `task-plan-generator`

Use when:

- a short-lived execution plan is needed for a specific coding task
- the user asks for a plan before implementation
- repository rules require a task plan artifact under `plan/`

Do not route there when:

- the work needs durable multi-session `PLANS.md` state
- product scope is still unclear
- the user needs immediate routing rather than a plan artifact

### Route to `codebase-recon`

Use when:

- the agent needs read-first orientation before changing an unfamiliar or risky code area
- local repo evidence such as structure, git churn, entry points, tests, or contracts should guide the next step

Do not route there when:

- the affected files are already obvious and small
- the task is architecture design rather than current-state reconnaissance

### Route to `agents-md-maintainer`

Use when:

- the repository lacks a durable `AGENTS.md`
- Codex keeps making repo-specific mistakes
- command, editing, or workflow guardrails should be written down before more work continues

Do not route there when:

- the blocker is missing feature scope, architecture, implementation, or validation

### Route to `plans-md-maintainer`

Use when:

- the work spans multiple sessions or milestones
- the current stage, blockers, and next actions should persist in `PLANS.md`
- execution state exists in chat or scattered docs and should be normalized into one durable plan

Do not route there when:

- the task is too small to justify persistent planning
- the need is immediate routing only, not plan maintenance

### Route to `bug-investigator`

Use when:

- a reported bug or failing behavior needs root-cause diagnosis before implementation
- symptoms, logs, screenshots, or failing tests exist but the fix is not obvious
- reproduction status or likely affected path is still unknown

Do not route there when:

- the fix is already known and implementation should proceed
- the issue is specifically CI provider, runner, or workflow failure

### Route to `codebase-migration-planner`

Use when:

- a migration, framework upgrade, dependency upgrade, API rename, or multi-file refactor needs batching
- the change needs blast-radius analysis, codemod strategy, validation gates, or rollback planning

Do not route there when:

- ordinary feature architecture is the blocker
- the change is small enough for direct implementation

### Route to `code-reviewer`

Use when:

- a completed or in-progress code change needs engineering review
- the user asks for a PR, patch, branch, or diff review
- the concern is correctness, maintainability, contracts, or missing validation

Do not route there when:

- the review is security-specific, regression-impact-specific, or deploy-readiness-specific

### Route to `feature-impact-reviewer`

Use when:

- a behavior-changing feature needs focused regression impact analysis
- the team needs to know what could break before implementation
- implementation is done but likely regression surfaces still need a tight review

Do not route there when:

- the change is purely cosmetic or obviously isolated
- the real need is broad architecture work or actual test execution

### Route to `ci-failure-triager`

Use when:

- CI, PR checks, GitHub Actions, or remote build/test jobs are failing
- logs must be collected and classified before deciding whether the fix is code, test, dependency, environment, or workflow config

Do not route there when:

- the failure is already reproduced locally and needs normal bug investigation
- the user only wants new tests or local validation

### Route to `pr-feedback-resolver`

Use when:

- the user wants PR review comments, requested changes, or issue feedback addressed
- comments need to be inventoried, grouped, selected, fixed, and replied to

Do not route there when:

- the user asks for a fresh code review rather than resolving supplied feedback
- the main issue is CI failure rather than review comments

### Route to `web-security-reviewer`

Use when:

- the work touches auth, permissions, tenant boundaries, sensitive data, uploads, rich text, callbacks, cookies, CORS, CSRF, or similar security-sensitive surfaces
- release confidence depends on a scoped application-security review

Do not route there when:

- the task is visual-only or otherwise has no meaningful security surface
- the request is really for infrastructure security architecture or formal penetration testing

## Non-Goals

- Replacing specialized skills with generic coordination prose
- Creating product scope, design, architecture, or implementation artifacts directly
- Bypassing missing gates without explanation
- Acting as a generic all-purpose manager for the whole project
