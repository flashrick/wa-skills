---
name: plans-md-maintainer
description: Create, audit, or update a project-level PLANS.md that persists stage status, milestones, blockers, accepted decisions, open questions, and next actions across Codex sessions for multi-step work.
---

# PLANS.md Maintainer

## Purpose

This skill creates and maintains a project-level `PLANS.md` for multi-step or long-running work.

Use this skill to:
- create an initial `PLANS.md` for a project or major feature once the work is real enough to track
- convert planning and handoff discussion into a durable execution plan
- keep stage status, milestones, blockers, decisions, and next actions current
- record workflow gates so later Codex sessions do not need to reconstruct state from chat
- update `PLANS.md` after major progress, blockers, or routing decisions

Do not use this skill to:
- replace product scoping
- replace architecture design
- replace implementation
- replace testing execution
- turn `PLANS.md` into a giant spec or retrospective narrative
- write ephemeral task notes that do not need to survive the current session

---

## What This Skill Owns

This skill owns the persistent execution plan for the current project or feature.

It should answer questions like:
- What is the current goal?
- What stage is the work in now?
- What has already been decided?
- What is blocked?
- What still needs a user decision?
- What should happen next?
- What gate conditions matter before the next stage can begin?

It does **not** own:
- repository operating rules
- detailed UX direction
- architecture authorship
- implementation work
- full test ownership
- deployment execution
- generic workflow routing without updating the plan artifact

---

## When This Skill Should Trigger

Trigger this skill when:
- the work is multi-step and should survive across sessions
- a long-running task needs a durable plan file
- the current stage, blockers, and next actions should be recorded explicitly
- a `workflow-orchestrator` routing or gate decision should be persisted
- an existing `PLANS.md` is stale after meaningful progress
- the user explicitly wants a `PLANS.md`
- several specialized skills need a shared execution artifact

Especially useful for:
- new projects once scope is clear enough to sequence
- major features with design, architecture, implementation, and validation stages
- gated work where later sessions need to know what was decided and what is still blocked
- ongoing efforts where chat history is not a reliable source of project state

---

## When This Skill Should NOT Trigger

Do not trigger this skill when:
- the task is tiny and likely to finish in one short step
- the user only wants immediate implementation with no ongoing coordination value
- the task belongs entirely to another skill and does not need persistent tracking
- there is no meaningful project or feature scope yet
- the user wants real-time routing advice but does not need a durable plan artifact

Examples that usually do **not** need this skill:
- a one-file bug fix
- a copy-only change
- a small visual tweak
- a local rename with no broader workflow impact

---

## Relationship To Neighbor Skills

Use this skill after or alongside nearby skills, not instead of them.

- `project-manager` defines kickoff scope, slices, and acceptance criteria.
- `plans-md-maintainer` persists the current execution state of that scoped work over time.
- `workflow-orchestrator` decides what should happen next when routing is unclear.
- `plans-md-maintainer` records that routing outcome in `PLANS.md` so it survives later sessions.

If the work is still too vague to plan durably, use `project-manager` first.

If the next step is unclear right now, use `workflow-orchestrator` first or together with this skill.

---

## Expected Inputs

This skill expects some or all of:
- current user goal
- relevant planning discussion
- existing `PLANS.md`, if present
- `AGENTS.md`, if present
- current project documents such as scope, acceptance criteria, architecture notes, UI docs, or test summaries
- `workflow-orchestrator` output, if available
- current blockers and open questions
- changed files or completed milestones if the plan is being updated mid-project

Helpful inputs:
- what has already been decided
- what is still unclear
- what stage the work is in
- what must happen next
- what counts as done for the next gate

---

## Expected Outputs

Primary output:
- a project-root `PLANS.md`

Secondary outputs:
- current stage updates
- milestone status updates
- newly recorded blockers
- newly recorded accepted decisions
- newly recorded open questions
- next recommended actions and next recommended skill

Optional:
- a short summary of what changed in `PLANS.md`

---

## Core Principles

### 1. Keep the plan durable

Write `PLANS.md` so a future Codex session can continue the work with minimal confusion.

### 2. Keep the plan concise

Do not turn `PLANS.md` into a giant requirements or architecture document.

Prefer:
- current state
- key decisions
- blockers
- next actions
- gate conditions

### 3. Record decisions explicitly

If something is approved or rejected, record it.

### 4. Record uncertainty explicitly

If something is unresolved, capture it as an open question, blocker, or assumption.

### 5. Update only after meaningful change

Update `PLANS.md` when:
- a stage changes
- a gate is passed or blocked
- a major decision is made
- a blocker is discovered or resolved
- implementation materially changes what comes next

Do not churn the plan for trivial noise.

---

## Preferred PLANS.md Structure

Prefer a structure like this unless the project clearly needs a simpler variant:

```md
# PLANS.md

## Goal
- what is being built or changed

## Current Stage
- current phase
- short status summary

## Milestones
- milestone list with status

## Accepted Decisions
- decisions already made

## Open Questions
- items requiring user or team decision

## Blockers
- current blockers and why they matter

## Next Skill
- the next recommended skill

## Next Actions
- immediate next steps

## Acceptance Gates
- what must be true before moving to the next phase

## Change Log
- short updates to this plan over time
```

Keep the file practical:
- prefer current truth over historical detail
- keep old change log items short
- remove stale sections instead of letting them accumulate

---

## Questioning Strategy

Ask only for information that materially changes the durable plan.

Prefer reading existing artifacts before asking the user to restate status.

Good questions include:
- "What is the latest completed stage or milestone?"
- "Which decision is still blocking the next step?"
- "Should this plan cover the whole project or only this feature?"
- "What condition must be true before the next stage is considered ready?"

Avoid questions that belong to other skills, such as:
- product discovery and requirement interviews
- detailed UX preference gathering
- architecture debates
- implementation design
- broad QA strategy

---

## Workflow

### 1. Determine Whether A Durable Plan Is Warranted

- Check whether the work is large enough, long enough, or uncertain enough to justify `PLANS.md`.
- If the task is too small, say so and avoid creating needless planning overhead.

### 2. Read Existing State

- Read the existing `PLANS.md`, if present.
- Read nearby artifacts that define current truth, such as kickoff docs, architecture notes, test summaries, and workflow status docs.
- Treat stale plan entries as candidates for correction, not as truth.

### 3. Normalize The Current Execution State

- Identify the current stage, completed milestones, blockers, open questions, and next actions.
- Distinguish confirmed decisions from assumptions.
- Keep the state grounded in current artifacts and recent progress.

### 4. Write Or Update `PLANS.md`

- Capture only the information needed for future continuation.
- Keep the plan concise and operational.
- Prefer one stable top-level plan file over fragmented status notes.

### 5. Remove Drift

- Delete stale next steps that are already done.
- Resolve or retire blockers that no longer apply.
- Mark superseded decisions clearly rather than letting contradictions stand.

### 6. Make The Next Step Obvious

- Name the next recommended skill or owner when useful.
- State the immediate next actions and gate conditions.
- Make it easy for the next session to start without reconstructing context.

---

## Handoff Expectations

### To `workflow-orchestrator`

Provide:
- the current stage and blockers as recorded in `PLANS.md`
- the unresolved gate or routing uncertainty
- the evidence already captured in the plan

Do not provide:
- a stale plan with unverified status
- generic "what next?" questions with no current-state summary

### To `project-manager`

Provide:
- the current goal and why scope is still too unclear for a stable plan
- the open questions blocking durable sequencing

Do not provide:
- pseudo-scope invented only to fill the plan

### To implementation, testing, or deploy skills

Provide:
- the current stage
- the exact next action or milestone
- known blockers, accepted decisions, and relevant gate conditions

Do not provide:
- vague status prose that hides what is actually ready

---

## Non-Goals

- Acting as a generic project manager for everything
- Replacing `project-manager` kickoff work
- Replacing `workflow-orchestrator` routing decisions
- Replacing implementation, testing, or deploy execution
- Preserving every historical detail instead of current actionable state
