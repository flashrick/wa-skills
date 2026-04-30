---
name: task-plan-generator
description: Create a short-lived execution plan for a specific coding task with scope, ordered action items, validation commands, risks, and open questions. Use when the user asks for a plan before implementation or the agent needs a local task plan, not for durable multi-session PLANS.md maintenance or product requirement scoping.
---

# Task Plan Generator

## What It Does

- Creates a concise execution plan for one coding task.
- Separates in-scope and out-of-scope work.
- Orders discovery, implementation, validation, and handoff steps.
- Captures risks and open questions without turning the plan into a long project document.

## When It Should Trigger

- The user asks for a plan, implementation plan, coding plan, or step-by-step approach.
- A coding task is concrete enough to sequence but not yet ready for edits.
- The repository requires a task plan artifact under `plan/`.
- A task needs short-term coordination but does not need a durable `PLANS.md`.

## When It Should Not Trigger

- The work needs product scope or acceptance criteria; use `project-manager`.
- The work spans multiple sessions and needs durable project state; use `plans-md-maintainer`.
- The next skill or stage is unclear; use `workflow-orchestrator`.
- The user asked for direct implementation and no plan artifact is required.

## Expected Inputs

- User request or task prompt.
- Relevant repository instructions.
- Existing docs, changed files, or likely touched areas.
- Known test commands or validation expectations.
- Any constraints such as deadlines, dependency restrictions, or approval gates.

## Expected Outputs

- A Markdown task plan, usually under `plan/` when a file is required.
- A short scope statement.
- Ordered action items.
- Validation commands or checks.
- Risks, assumptions, and open questions.

## Workflow

### 1. Scan Context

- Read repository instructions and obvious docs first.
- Identify likely touched files, commands, and constraints.
- Do not perform deep implementation research unless needed for a reliable plan.

### 2. Define Scope

- State what is in scope and out of scope.
- Mark assumptions clearly.
- Keep deferred work visible instead of hiding it in prose.

### 3. Create Ordered Actions

- Write verb-first, atomic steps.
- Order work as discovery, edits, validation, documentation, and handoff.
- Include at least one validation step.
- Include risk or edge-case handling when the task can affect behavior.

### 4. Limit Questions

- Ask only questions that block responsible planning.
- Prefer at most three open questions.
- If a reasonable assumption is safe, state it and continue.

## Output Shape

```md
# Task Plan

## Goal
- What will be done and why

## Scope
- In
- Out

## Action Items
- [ ] Discover...
- [ ] Update...
- [ ] Verify...

## Validation
- Commands or checks

## Risks And Edge Cases
- Concrete risk or assumption

## Open Questions
- Question, only if blocking or important
```

## Relationship To Neighbor Skills

- Use `project-manager` when the task itself is still unclear.
- Use `plans-md-maintainer` when the plan must persist across sessions.
- Use `workflow-orchestrator` when the main question is which skill should act next.
- Use implementation skills only after the plan is accepted or the task is ready for edits.

## Non-Goals

- Maintaining long-running project state
- Replacing architecture or UX design
- Writing implementation code
- Acting as a generic project manager
- Creating overly detailed plans that duplicate downstream skill work
