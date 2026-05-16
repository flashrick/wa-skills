---
name: safe-change-planner
description: Plan a safe change to legacy or weakly tested code by defining behavior to preserve, characterization coverage, seams, and refactor-vs-change boundaries before implementation starts.
---

# Safe Change Planner

## What It Does

- Prepares a risky existing-code change before implementation starts.
- Defines the behavior that must stay, the behavior that should change, and the smallest safe path between them.
- Identifies characterization-test targets, seam options, and where preparatory refactoring is justified.
- Produces a change-safety plan that implementation and test skills can follow without mixing discovery, redesign, and behavior change blindly.

## When It Should Trigger

- The code to be changed is legacy, weakly tested, tightly coupled, or expensive to modify safely.
- A feature or bug fix requires structural preparation before the actual behavior change.
- A refactor is needed, but the team must preserve current observable behavior while reducing local change risk.

## When It Should Not Trigger

- The main problem is still unknown and needs diagnosis first; use `bug-investigator`.
- The task is a broad migration or mechanical multi-file refactor; use `codebase-migration-planner`.
- The code area is already well tested, well isolated, and safe for direct implementation.
- The request is for direct coding, not change-planning.

## Expected Inputs

- Requested behavior change or known bug fix target
- Existing behavior that must remain intact, if known
- Relevant code paths, tests, logs, or investigation notes
- Current implementation constraints such as globals, framework hooks, hidden dependencies, or expensive setup

## Expected Outputs

- `docs/change-safety-plan.md`
- A clear statement of behavior to preserve and behavior to change
- Recommended characterization or other explicit observation points
- The smallest useful seam and dependency-breaking strategy
- Handoff notes for `backend-implementer`, `frontend-implementer`, and `test-engineer`

## Workflow

### 1. Define The Safety Target

- Restate the requested change in terms of current behavior and intended behavior.
- Separate facts from guesses.
- Make uncertainty explicit instead of silently converting it into a design decision.

### 2. Inspect The Change Surface

- Identify the likely change point, nearby collaborators, and blocking dependencies.
- Note whether the main risk is missing tests, hidden construction, global state, framework coupling, or unclear behavior.
- Keep the scope local unless evidence shows the change truly crosses a wider boundary.

### 3. Decide What Must Be Observed First

- Choose the smallest useful characterization or observation path before changing semantics.
- Prefer focused checks close to the change point when possible.
- Use broader integration coverage only when it is the safest first way to observe current behavior.

### 4. Choose The Smallest Useful Seam

- Identify the seam needed for sensing, separation, or both.
- Prefer narrow dependency-breaking moves over broad redesign.
- Record temporary techniques that need cleanup after the change becomes safe.

### 5. Split The Work Intentionally

- Separate preparatory refactoring, behavior change, and follow-up cleanup.
- Keep each step reviewable and reversible.
- Stop planning once the requested change can proceed safely without speculative redesign.

### 6. Prepare Handoff

- Route the implementation to `backend-implementer` or `frontend-implementer`.
- Route characterization and regression coverage to `test-engineer`.
- If the code is still too poorly understood, send it back to `bug-investigator` with the missing evidence called out.

## Output Shape

When producing a change-safety artifact, prefer this structure:

```md
# Change Safety Plan

## Target Change
- Requested behavior change
- Behavior that must remain unchanged

## Current Risks
- Why this area is hard to change safely

## Observation Plan
- Characterization tests or other explicit observation points

## Seam Strategy
- Smallest seam to introduce or use
- Dependency-breaking notes

## Step Plan
- Preparatory refactor
- Behavior change
- Follow-up cleanup

## Handoff Notes
- Implementation owner
- Test owner
- Open uncertainty
```

## Handoff Expectations

### To `backend-implementer` or `frontend-implementer`

Provide:

- the exact behavior to preserve
- the intended behavior delta
- the recommended seam or preparatory refactor
- any dependency-breaking constraint that should not grow during implementation

Do not provide:

- a broad rewrite disguised as safety work
- speculative architecture beyond the requested change

### To `test-engineer`

Provide:

- which checks are characterization tests versus post-change regression tests
- the smallest useful fixtures, setup, or environment detail
- what behavior is still uncertain and therefore must be observed before semantics change

Do not provide:

- a generic request to add coverage everywhere
- an implementation task hidden as test planning

## Non-Goals

- Root-cause diagnosis when the cause is still unknown
- Full migration planning across many modules
- Doing the implementation directly
- Acting as a general architecture skill
