---
name: feature-impact-reviewer
description: Review the regression impact of a planned or implemented feature on existing behavior, edge cases, integration points, and shared contracts. Use this skill for focused change-impact analysis on related files, not full-repo auditing, broad architecture design, or primary feature implementation.
---

# Feature Impact Reviewer

## Purpose

This skill performs a focused impact review for a specific feature change.

Use this skill to:
- identify which existing behaviors may be affected by a change
- surface easy-to-miss dependencies and edge cases
- highlight non-obvious regression risks
- suggest the most important tests or verification points
- create a short impact summary before or after implementation

Do not use this skill to:
- scan the entire repository by default
- replace architecture planning
- replace implementation
- replace full testing ownership
- rewrite long project documents
- act as a generic code reviewer for unrelated changes

---

## What This Skill Owns

This skill owns **change-specific impact awareness**.

It should answer questions like:
- What existing modules or behaviors could this feature affect?
- What assumptions must remain true after the change?
- What edge cases are easy to forget?
- What tests should be added or updated first?
- What old behavior might break even if the new feature works?

It does **not** own:
- repo-wide operating rules
- product scope definition
- visual design direction
- full architecture design
- broad code quality review unrelated to the feature
- deployment readiness

---

## When This Skill Should Trigger

Trigger this skill when:
- a feature modifies existing behavior
- a change touches shared logic, shared state, or shared data structures
- a change affects existing APIs, defaults, permissions, workflows, or integrations
- the task is likely to create regressions outside the main happy path
- the user explicitly asks what could break
- a feature is implemented and needs a quick regression-oriented review

Especially useful for:
- editing existing flows
- changing API request/response shape
- changing default values or validation rules
- changing auth, role, or permission logic
- changing shared UI components or shared backend services
- changing database fields that other features depend on

---

## When This Skill Should NOT Trigger

Do not trigger this skill when:
- the task is a tiny isolated cosmetic change
- the task is a pure text or copy update
- the task is a very local rename with no behavior change
- the task is an unrelated full-repo audit
- the task belongs primarily to planning, architecture, implementation, or full test execution
- there is no concrete feature or change set to analyze

Examples that usually do **not** need this skill:
- changing button text only
- adjusting spacing only
- renaming a private variable in one file only
- adding a brand new isolated static page with no shared logic

---

## Scope Control Rules

This skill must stay low-cost and focused.

By default:
- inspect only the files directly related to the requested feature
- inspect nearby dependencies only when clearly relevant
- inspect `AGENTS.md` if it exists
- inspect related tests only if they are likely to reveal affected behavior
- do not perform a full repository scan unless explicitly requested

If the candidate impact area becomes too large:
- stop expanding scope automatically
- summarize the uncertainty
- recommend a narrower follow-up review

---

## Inputs Expected

This skill expects some or all of:
- the current feature request
- changed files, if implementation already exists
- likely related files
- existing `AGENTS.md`, if present
- current acceptance criteria, if available
- relevant API or data contract docs, if available
- relevant tests, if available

Helpful inputs:
- "What changed"
- "What is supposed to remain unchanged"
- "Which user flow this belongs to"
- "Which endpoints, models, or components are involved"

---

## Expected Outputs

Primary output:
- a short impact review focused on the current feature

Optional file:
- `docs/feature-impact.md`

If writing `docs/feature-impact.md`, keep it short and practical.

Preferred sections:
- changed area
- likely impacted areas
- must-preserve behavior
- edge cases
- suggested tests
- open uncertainty

---

## Output Format

Prefer compact output.

Use a structure like:

### Impact Summary
- what is being changed
- what areas are most likely affected

### Must-Preserve Behavior
- existing behaviors that should not break

### Edge Cases To Check
- high-risk edge cases only

### Suggested Verification
- the most important tests or manual checks

### Open Uncertainty
- what remains unclear because of missing context

Keep the output concise.
Prefer 5–10 strong points over a long report.

## Handoff Expectations

### To `backend-implementer` or `frontend-implementer`

Provide:
- must-preserve behavior that the implementation must keep intact
- the highest-risk dependencies or shared contracts touched by the change
- edge cases that should be handled in code, not just tested later

Do not provide:
- vague warnings with no concrete impacted behavior
- architecture redesign requests unless the change clearly exposed a structural problem

### To `test-engineer`

Provide:
- the smallest set of regression targets most likely to fail
- concrete behaviors, states, permissions, defaults, or contracts worth validating
- any uncertainty that needs tests to confirm or rule out

Do not provide:
- a generic request to "add more coverage"
- full test execution ownership

### To `workflow-orchestrator`

Provide only when routing is the real blocker:
- whether impact review should happen before implementation, after implementation, or both
- which downstream skill should act next based on the current level of risk and evidence

---

## Review Modes

This skill supports two modes.

### Mode A: Pre-Implementation Review
Use before coding.

Goal:
- identify risk before implementation starts

Focus on:
- affected modules
- hidden dependencies
- compatibility constraints
- must-preserve behavior
- likely test targets

### Mode B: Post-Implementation Review
Use after coding.

Goal:
- identify what may still be missing after implementation

Focus on:
- changed files
- unhandled edge cases
- missing regression checks
- integration risks
- likely incomplete coverage

If the user does not specify the mode:
- infer it from context
- default to the most useful mode

---

## Review Priorities

When reviewing impact, prioritize in this order:

1. existing user-visible behavior that may regress
2. shared contracts such as API shape, defaults, schema, or permissions
3. integration points with other modules
4. edge cases involving empty states, null values, missing data, retries, or invalid input
5. tests that are most likely to catch the regression early

Do not spend most of the budget on low-risk stylistic observations.

---

## Workflow

### 1. Identify The Exact Change
Determine:
- what feature or behavior is being added, changed, or fixed
- whether this is pre-implementation or post-implementation review

If the change is too vague, ask for the minimum clarification needed.

### 2. Read Only Relevant Context
Inspect:
- the directly affected files
- closely related files
- `AGENTS.md` if present
- related tests if clearly relevant

Do not widen scope unless there is strong evidence of another affected area.

### 3. Trace Likely Impact
Identify:
- dependent modules
- reused functions or components
- shared schemas or contracts
- user flows that may rely on the same behavior
- implicit assumptions that could be broken

### 4. Extract Must-Preserve Behavior
List the existing behaviors that should remain correct after the change.

These should be concrete, not generic.

### 5. Identify High-Risk Edge Cases
Focus only on edge cases with realistic regression risk.

Avoid generating a giant exhaustive list.

### 6. Suggest Verification
Recommend the smallest set of tests or checks that would catch the most likely regressions.

Prefer targeted checks over broad "test everything" advice.

### 7. Summarize Clearly
Produce a short, actionable impact review.

If uncertainty remains, say what is unclear and why.

---

## Questioning Strategy

Ask only if needed.

Good questions:
- "Which existing flow should remain unchanged?"
- "Should I review this before implementation or after the changes?"
- "Which files or module area should I treat as the main change surface?"

Avoid broad questions that belong to other skills, such as:
- full product requirement interviews
- broad architecture debates
- design preference collection
- full test strategy planning

## Constraints

- Stay focused on the current change.
- Default to related-file review, not full-repo review.
- Prefer actionable findings over long narrative.
- Distinguish confirmed impact from likely-but-unconfirmed risk.
- Do not invent dependencies without evidence.
- Do not replace the test engineer.
- Do not drift into general code review unless it directly affects the current feature.

---

## Quality Bar

A good result:
- is tied to a concrete feature or change
- stays narrowly focused
- identifies realistic regression risks
- names specific behaviors to preserve
- suggests a few strong verification points
- avoids full-repo overreach
- is short enough to remain cost-effective

A bad result:
- scans everything
- produces generic warnings
- ignores current repo context
- repeats architecture advice
- lists dozens of speculative edge cases
- acts like a full implementation or testing skill

## Non-Goals

- Acting as a full-repo risk auditor
- Replacing `repo-architect` for major structural design
- Replacing `test-engineer` for writing and running tests
- Acting as a generic code reviewer for unrelated changes
