---
name: agents-md-maintainer
description: Create, audit, or update a repository-level AGENTS.md based on the actual codebase, commands, workflow rules, and editing boundaries. Use this skill when a repo needs durable Codex operating guidance, not product planning, architecture design, or feature implementation.
---

# AGENTS.md Maintainer

## Purpose

This skill creates or maintains a project-level `AGENTS.md` that changes how Codex should operate inside a specific repository.

Use this skill to:
- create a new repository-level `AGENTS.md`
- audit an existing `AGENTS.md` against the real codebase
- remove stale, vague, or contradictory repo guidance
- encode stable repo-specific operating rules for Codex
- document command usage, editing boundaries, workflow guardrails, and non-goals

Do not use this skill to:
- define product requirements
- define UX direction
- define system architecture
- implement features
- own testing execution
- own deployment execution
- write generic prompting advice detached from a real repository

---

## What This Skill Owns

This skill owns the repository operating contract for Codex.

It should define:
- how Codex should navigate this repo
- which commands Codex should prefer
- which paths are safe or unsafe to modify
- which workflow rules are durable and worth preserving
- which actions should be avoided
- what this repo-level file should explicitly not try to control

It should not replace:
- product specs
- architecture docs
- onboarding docs
- test plans
- deployment runbooks

---

## When This Skill Should Trigger

Trigger this skill when:
- the user wants to create a project-level `AGENTS.md`
- the repository already has `AGENTS.md` but it is outdated, vague, generic, or contradictory
- Codex has repeated repo-specific mistakes that suggest missing local operating guidance
- the team wants repo-specific guardrails for commands, editing, workflow, or path boundaries
- the repository has enough local context to infer durable operating rules
- implementation or repository evolution changed commands, directory boundaries, generated areas, validation expectations, or workflow rules enough that existing `AGENTS.md` is now stale

---

## When This Skill Should NOT Trigger

Do not trigger this skill when:
- the task is to define product scope, UX, architecture, or implementation details directly
- the user only wants abstract advice about prompt writing
- the request is about global behavior across all repositories rather than repo-local guidance
- there is no meaningful repository context and the user does not want a repo-grounded draft
- the task belongs to another narrow skill such as planning, architecture, testing, or deployment readiness
- the change is only about product or developer documentation such as `README.md` and does not alter how Codex should operate in the repository

---

## Expected Inputs

This skill expects some or all of:
- the current repository or target project directory
- the existing `AGENTS.md`, if present
- root documentation such as `README.md`
- package manifests or workspace files
- test, lint, build, and CI configuration
- important app, service, or package directories
- user-provided team rules or editing boundaries, when available

Examples:
- `README.md`
- `package.json`
- `pnpm-workspace.yaml`
- `pyproject.toml`
- `Makefile`
- CI config
- test config
- lint config
- deployment or operational notes
- existing repo docs that describe workflow or ownership

---

## Expected Outputs

Primary output:
- a project-root `AGENTS.md` that is specific enough to materially change how Codex behaves in this repository

Secondary outputs:
- removal or correction of stale, vague, or contradictory repo rules
- targeted updates that keep `AGENTS.md` aligned after meaningful repository changes
- a short change summary describing:
  - what was added
  - what was removed
  - what was inferred from the repo
  - what remains uncertain or was left out

Optional:
- brief notes explaining assumptions if the repo does not provide enough evidence for a fully confident rule

---

## Rule Selection Criteria

Only include a rule in `AGENTS.md` if at least one of the following is true:
- it changes how Codex should edit, test, build, or navigate the repository
- it prevents a recurring mistake
- it defines a stable workflow boundary
- it protects risky, generated, or restricted areas
- it reflects a real command, path, convention, or operational constraint found in the repository
- it provides a durable repo-level rule that future Codex sessions should follow

Do not include rules that are:
- generic advice already covered by Codex defaults
- temporary sprint instructions
- speculative architecture preferences not grounded in the repo
- issue-specific tasks that belong in a plan rather than `AGENTS.md`
- broad product or design decisions better owned by other skills

---

## Repository Inspection Order

Before drafting or revising `AGENTS.md`, inspect the repository in this order when available:

1. root `README.md`
2. root package manager or build metadata
   - `package.json`
   - `pnpm-workspace.yaml`
   - `turbo.json`
   - `pyproject.toml`
   - `go.mod`
   - `Cargo.toml`
   - equivalent project files
3. CI and automation files
4. lint, format, test, and build configuration
5. top-level app, service, package, or module directories
6. existing docs describing workflow, ownership, architecture, or release process
7. existing `AGENTS.md`

Do not treat the existing `AGENTS.md` as authoritative until it has been checked against the real repo.

---

## Command Verification Rules

Only write a command into `AGENTS.md` if one of the following is true:
- the command exists in the repository
- the command is clearly documented in repository files
- the command was explicitly provided by the user

Never present an inferred command as confirmed fact.

If a command seems likely but cannot be verified:
- omit it, or
- include it only as a clearly labeled assumption

Prefer exact commands over generic wording.

Prefer:
- `Run \`pnpm test\` from \`frontend/\``

Over:
- `run tests as appropriate`

---

## Questioning Strategy

Ask only for information that cannot be safely inferred from the repository.

Prefer inspecting local files before asking how the project works.

If the repo contains multiple apps or services, ask only the minimum needed to determine:
- whether `AGENTS.md` should govern the whole repo or a subproject
- whether there are hard editing boundaries or forbidden directories
- whether there are team rules not visible in the codebase

If missing information does not block a useful first draft:
- continue
- label uncertain items as assumptions
- avoid inventing process

Good questions include:
- "Should this `AGENTS.md` govern the whole monorepo or just one app?"
- "Are there any commands or directories that Codex must never touch?"
- "Do you want strict workflow sequencing here, or mainly editing and command guardrails?"

Avoid questions that belong to other skills, such as:
- product requirement clarification
- detailed UX preference gathering
- architecture debates
- implementation planning
- test strategy design
- deployment strategy design

---

## Workflow

### 1. Inspect The Real Repository

Read the repo before writing rules.

Check the files and directories that reveal:
- how the project is structured
- how it is run
- how it is built
- how it is tested
- which areas are generated, risky, or boundary-sensitive

If `AGENTS.md` already exists, treat it as an auditable artifact, not ground truth.

### 2. Identify Behavior-Critical Rules

Capture only rules that materially change how Codex should behave in this repository.

Prioritize rules about:
- key directories
- real commands
- safe and unsafe edit zones
- preferred workflow order
- validation expectations
- destructive or prohibited actions
- generated-file handling
- repo-specific constraints that are likely to recur

### 3. Separate Stable Rules From Temporary Instructions

Keep durable repo rules in `AGENTS.md`.

Do not encode:
- sprint-specific tasks
- temporary deadlines
- one-off issue notes
- speculative future process

If a rule is uncertain, either:
- leave it out, or
- label it as an assumption

### 4. Write Concrete Operational Guidance

Use direct, operational language.

Name exact:
- commands
- paths
- directories
- forbidden actions
- review expectations
- workflow boundaries

Prefer small, accurate instructions over large generic sections.

### 5. Keep The File Easy To Maintain

Use a stable structure.

Remove:
- duplicates
- vague rules
- contradictions
- filler advice

The file should be easy to update as the repo evolves.

### 6. Validate The Draft Against The Repo

Re-read the drafted `AGENTS.md` and compare it against the current repository.

Correct:
- wrong commands
- wrong paths
- invented workflow claims
- unsupported assumptions

Prefer a smaller and accurate file over a larger speculative one.

When the prompt follows implementation work, check whether the code changes altered:
- how the repo is run
- how it is tested
- which paths are safe to edit
- which generated or protected areas exist
- which workflow rules Codex now needs to follow

If none of those changed, do not force an `AGENTS.md` edit.

### 7. Produce A Change Summary

After creating or updating the file, summarize:
- added rules
- removed rules
- corrected rules
- inferred assumptions
- unresolved uncertainty

---

## Preferred Output Shape

When creating or rewriting a project-level `AGENTS.md`, prefer a structure like this:

```md
# AGENTS.md

## Repository Purpose
- what this repo is for
- what kind of changes Codex should expect to make

## Operating Boundaries
- in-scope areas
- out-of-scope or protected areas
- destructive actions to avoid

## Key Paths
- important apps, packages, services, or docs
- generated or vendor-managed areas

## Commands
- install
- dev
- test
- lint
- build

## Workflow Rules
- preferred order of work
- required checks before handoff
- review expectations

## Editing Constraints
- file-specific guardrails
- generated code handling
- migration or schema constraints
- directories that require caution

## Non-Goals
- what this file intentionally does not try to control
