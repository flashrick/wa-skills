---
name: codebase-migration-planner
description: Plan large codebase migrations, dependency upgrades, framework upgrades, API renames, or multi-file refactors in reviewable batches with blast-radius analysis, codemod strategy, validation gates, and rollback notes. Use before executing broad mechanical or structural changes, not for ordinary feature architecture or one-off implementation.
---

# Codebase Migration Planner

## What It Does

- Plans large migrations and multi-file refactors before implementation starts.
- Defines the exact transform, affected files, batch boundaries, validation gates, and rollback strategy.
- Helps avoid unreviewable changes by separating discovery, codemod design, manual edits, and verification.
- Produces a migration plan that implementation agents can execute in slices.

## When It Should Trigger

- The task is a framework, runtime, package, or toolchain upgrade.
- A public API, helper, config format, import path, or data contract must be renamed across many files.
- A repo structure change affects multiple modules or packages.
- A broad refactor needs reviewable batches and clear acceptance gates.
- The user asks how to safely migrate, upgrade, rename, or codemod a codebase.

## When It Should Not Trigger

- The task is a normal feature architecture decision; use `repo-architect`.
- The change is isolated to a small number of files and can be implemented directly.
- The user needs the migration executed immediately and the transform is already fully specified.
- The work is mainly deployment readiness, testing, or bug diagnosis.

## Expected Inputs

- Target migration or upgrade.
- Current stack and package/runtime versions, if known.
- Existing tests and CI commands.
- Search patterns, APIs, imports, configs, or files expected to change.
- Release constraints, compatibility requirements, or rollback expectations.

## Expected Outputs

- `docs/migration-plan.md` when a durable artifact is useful.
- Transform definition with in-scope and out-of-scope changes.
- Blast-radius estimate and affected file groups.
- Batch plan with ownership boundaries.
- Codemod or manual-edit strategy.
- Validation gates for each batch and final verification.
- Rollback and conflict-handling notes.

## Workflow

### 1. Define The Transform Precisely

- Turn vague migration goals into explicit transformations.
- Separate mechanical changes from behavior changes.
- State what must not change.
- Refuse to mix unrelated migrations in the same plan.

Examples:

- Good: replace `jest.fn()` with `vi.fn()`, convert `jest.mock()` to `vi.mock()`, and update test setup imports.
- Too broad: migrate all tests to a new stack and clean up old test utilities.

### 2. Measure Blast Radius

- Use fast searches such as `rg` to identify candidate files and patterns.
- Group files by package, app area, language, ownership, or risk.
- Identify generated files, vendored files, migrations, snapshots, and lockfiles that need special handling.
- Call out unknowns where static search may miss dynamic references.

### 3. Choose A Change Mechanism

- Prefer structured tools when syntax matters: AST tools, codemods, language-aware refactors, or formatter-supported rewrites.
- Use regex only for simple, unambiguous text transforms.
- Plan manual review for edge cases that codemods cannot safely handle.
- Keep codemod output and hand edits distinguishable in notes or commits.

### 4. Batch The Work

- Keep each batch reviewable and independently testable.
- Use one transform per batch or PR.
- Avoid mixing formatting-only churn with semantic migration.
- Define how completed files are tracked so later batches do not repeat work.
- Rebase or refresh open batches before stacking conflicting edits.

### 5. Define Validation Gates

- Pick focused commands for each batch.
- Define a full-suite or full-build gate for the final batch.
- Include static checks, type checks, lint, tests, generated artifacts, and smoke checks only when relevant.
- Name compatibility checks for public APIs, persisted data, or external clients.

### 6. Plan Rollback

- Ensure each batch can be reverted independently.
- Identify irreversible steps such as database migrations, data rewrites, or dependency lockfile changes.
- Plan feature flags, compatibility shims, or dual-read/write behavior when needed.

## Output Shape

```md
# Migration Plan

## Goal
- Target migration and reason

## Scope
- In
- Out

## Transform
- Exact mechanical and behavioral changes

## Blast Radius
- Search patterns
- Affected groups
- Unknowns

## Batches
- Batch order and boundaries

## Validation Gates
- Per-batch checks
- Final checks

## Rollback
- Revert strategy and compatibility notes

## Handoff Notes
- Which implementer or reviewer acts next
```

## Handoff Expectations

### To `repo-architect`

Route there when:

- module boundaries or ownership are still unclear
- the migration changes long-term repository structure
- interface seams need design before transforms can be specified

### To Implementation Skills

Provide:

- the exact batch boundary
- transform rules
- validation commands
- known edge cases and files to avoid

### To `test-engineer`

Provide:

- per-batch and final validation expectations
- high-risk compatibility behavior
- test fixtures or smoke checks needed after migration

### To `code-reviewer`

Provide:

- the intended transform
- known manual edits
- compatibility promises
- areas where codemod safety is uncertain

## Non-Goals

- Executing all migration batches by default
- Owning ordinary feature architecture
- Replacing implementation skills
- Replacing CI failure triage
- Creating external tracking issues or PRs unless another workflow explicitly owns that
