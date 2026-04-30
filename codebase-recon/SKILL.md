---
name: codebase-recon
description: Analyze an existing codebase before changes by mapping entry points, hotspots, recent churn, ownership signals, dependencies, and likely risk areas using local repo evidence. Use when Codex needs a read-first orientation or risk map before implementation, migration, review, or debugging.
---

# Codebase Recon

## What It Does

- Builds a practical map of an unfamiliar or risky code area before edits.
- Uses local evidence such as `git`, `rg`, tests, docs, dependency files, and module structure.
- Identifies likely entry points, high-churn files, shared contracts, test coverage, and risky dependencies.
- Produces a reading and change-risk map for downstream skills.

## When It Should Trigger

- The user asks to understand a codebase or area before coding.
- The task touches unfamiliar modules and needs orientation.
- A risky change needs hotspot, dependency, or ownership awareness before planning.
- A migration, bug investigation, or review needs a local evidence map first.
- The agent should decide where to read before making changes.

## When It Should Not Trigger

- The task is already scoped to a small, known file.
- The user needs architecture design for a future structure; use `repo-architect`.
- The user needs regression impact for a specific change; use `feature-impact-reviewer`.
- The user needs a root-cause diagnosis for a concrete bug; use `bug-investigator`.
- The user needs implementation, test writing, or deployment review.

## Expected Inputs

- Repository path and task goal.
- Suspected modules, files, commands, or symptoms if available.
- Time or scope constraints.
- Existing docs or architecture notes.

## Expected Outputs

- A concise codebase reconnaissance summary.
- Entry points and important files to read first.
- Hotspot or churn signals from local git history when available.
- Dependency and contract notes.
- Test and validation map.
- Handoff recommendations for the next skill.

## Workflow

### 1. Establish The Recon Target

- Restate the code area or task that needs orientation.
- Keep the scan bounded to the target unless the user asks for a broad map.
- Read repository instructions before interpreting structure.

### 2. Inventory Structure

- Use `rg --files` and directory listings to identify apps, packages, services, tests, docs, and config.
- Read README, architecture docs, or package manifests only as needed.
- Identify likely entry points such as routes, commands, workers, pages, services, or handlers.

### 3. Gather Local Risk Signals

- Use git history when available to inspect recent churn and frequently changed files.
- Check related tests and fixtures.
- Look for shared contracts: API schemas, data models, event names, generated types, config, and public helpers.
- Identify dependencies on external services, runtime env vars, or build tools.

### 4. Build A Reading Map

- Prioritize files that explain behavior, not every file in the area.
- Separate must-read files from optional references.
- Note where dynamic behavior may hide static references, such as reflection, config-driven routing, generated code, or dependency injection.

### 5. Hand Off

- Route to `task-plan-generator` when an execution plan is needed.
- Route to `codebase-migration-planner` when the recon reveals broad mechanical change.
- Route to `bug-investigator` when there is a concrete failing behavior.
- Route to `code-reviewer` when recon is for reviewing an existing diff.

## Output Shape

```md
# Codebase Recon

## Target
- Area or task being oriented

## Structure
- Relevant directories and entry points

## Read First
- Files or docs in priority order

## Risk Signals
- Churn, shared contracts, dependency, or coverage risks

## Validation Map
- Tests or commands likely to matter

## Handoff
- Recommended next skill and why
```

## Constraints

- Prefer `rg` and local git evidence over broad speculative summaries.
- Do not read the entire repository by default.
- Do not modify files.
- Do not invent ownership or architecture facts not supported by repository evidence.

## Non-Goals

- Designing future architecture
- Implementing the change
- Running full test suites unless needed for orientation
- Replacing targeted bug investigation
- Replacing focused regression impact review
