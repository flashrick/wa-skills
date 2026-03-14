# WA-SKILLS

A reusable **Agent Skills** library for **Codex CLI** web-project workflows.

This repository contains a set of narrow, role-based skills designed to help structure and execute a web project through staged collaboration.

The skills are intentionally separated by responsibility so they can be invoked explicitly or discovered more reliably through their metadata.

---

## Purpose

This repository is a **skills library**, not a product application.

It is designed to support a staged web-development workflow where different skills handle different types of work, such as:

- requirement clarification
- UI/UX design
- technical architecture
- backend implementation
- frontend implementation
- testing and validation
- deployment readiness
- workflow orchestration

The goal is to keep each skill:
- narrow
- reusable
- practical
- easy to trigger correctly
- easy to hand off to the next stage

---

## Skill List

### 1. `project-manager`
Responsible for:
- clarifying requirements
- breaking the project into smaller tasks
- asking targeted questions
- confirming scope, business logic, and tech stack
- creating project planning documents

Typical outputs:
- `docs/project-scope.md`
- `docs/todo.md`
- `docs/acceptance-criteria.md`

Use this skill when:
- the project is still vague
- requirements are incomplete
- the stack is undecided
- the work needs to be broken into stages

Do not use this skill for:
- direct UI implementation
- backend coding
- frontend coding
- testing
- deployment review

---

### 2. `uiux-designer`
Responsible for:
- visual direction
- layout structure
- design system direction
- component appearance
- interaction style
- page structure

Typical outputs:
- `docs/ui-style-guide.md`
- `docs/page-structure.md`
- `docs/component-spec.md`

Use this skill when:
- color, style, or layout needs to be decided
- pages and components need visual rules
- the interaction feel should be defined before implementation

Do not use this skill for:
- business logic planning
- architecture design
- frontend implementation
- backend implementation

---

### 3. `repo-architect`
Responsible for:
- technical architecture
- module boundaries
- API contracts
- data model direction
- implementation sequencing
- frontend/backend integration planning

Typical outputs:
- `docs/architecture.md`
- `docs/api-contract.md`
- `docs/data-model.md`

Use this skill when:
- the project needs technical structure
- the implementation path is unclear
- frontend/backend boundaries need to be defined
- contracts should be locked before coding

Do not use this skill for:
- requirement interviews
- visual preference collection
- final feature implementation
- deployment validation

---

### 4. `backend-implementer`
Responsible for:
- backend code
- APIs
- database models and migrations
- service logic
- middleware
- server-side integrations

Typical outputs:
- backend code changes
- migrations
- implementation notes when needed

Use this skill when:
- server-side behavior needs to be implemented
- database or API changes are required
- backend contracts must be realized in code

Do not use this skill for:
- redefining scope
- UI design decisions
- frontend rendering work
- deployment readiness review

---

### 5. `frontend-implementer`
Responsible for:
- pages
- components
- routes
- client-side state
- forms and interactions
- frontend API integration

Typical outputs:
- frontend code changes
- page and component implementation
- implementation notes when needed

Use this skill when:
- UI should be implemented in code
- routes and components need to be built
- client-side integration with backend is required

Do not use this skill for:
- collecting design preferences
- redefining project scope
- backend implementation
- deployment review

---

### 6. `test-engineer`
Responsible for:
- adding or updating tests
- running validation checks
- triaging failures
- identifying regressions
- reporting validation status
- highlighting coverage gaps

Typical outputs:
- updated test files
- `docs/test-report.md` when useful

Use this skill when:
- the implementation needs validation
- tests should be added or fixed
- failures need classification
- release confidence is unclear

Do not use this skill for:
- main feature planning
- UI direction
- core implementation ownership
- deployment environment review

---

### 7. `deploy-readiness-check`
Responsible for:
- deployment readiness review
- environment/config completeness
- build and release assumptions
- identifying release blockers
- creating a deployment checklist

Typical outputs:
- `docs/deploy-checklist.md`
- `docs/release-blockers.md` when useful

Use this skill when:
- the project is approaching release
- environment and build assumptions need review
- deployment blockers must be identified

Do not use this skill for:
- writing core feature code
- clarifying requirements
- acting as the main testing skill

---

### 8. `workflow-orchestrator`
Responsible for:
- deciding which skill should act next
- enforcing stage order
- checking handoff completeness
- verifying stage acceptance conditions
- keeping the project workflow disciplined

Typical outputs:
- `docs/workflow-status.md` when useful
- routing and handoff decisions

Use this skill when:
- the next stage is unclear
- the project needs workflow control
- a handoff should be checked
- stage readiness must be verified

Do not use this skill for:
- direct requirement interviews
- direct UI design work
- direct coding
- direct testing execution
- direct deployment analysis

---

## Recommended Stage Order

Default web-project workflow:

1. `project-manager`
2. `uiux-designer`
3. `repo-architect`
4. `backend-implementer`
5. `frontend-implementer`
6. `test-engineer`
7. `deploy-readiness-check`

`workflow-orchestrator` exists across the whole process and should be used to:
- determine the next step
- verify whether a stage is complete
- prevent premature execution
- maintain handoff discipline

---

## Recommended Usage

### Recommended starting point
For a new project, start with:

- `$project-manager`

This is the best default when the project is still being defined.

### When to use `$workflow-orchestrator`
Use:

- `$workflow-orchestrator`

when:
- multiple stage documents already exist
- the next action is unclear
- a stage handoff should be checked
- the project needs execution control rather than new content generation

### Typical explicit invocation pattern

Examples:

- `$project-manager` for requirement clarification
- `$uiux-designer` for visual system and layout direction
- `$repo-architect` for technical planning
- `$backend-implementer` for server-side implementation
- `$frontend-implementer` for UI implementation
- `$test-engineer` for validation
- `$deploy-readiness-check` for release preparation
- `$workflow-orchestrator` for gating and routing

---

## Design Principles

### 1. Narrow descriptions
Each skill is intentionally narrow.

This improves:
- discoverability
- trigger quality
- handoff clarity
- maintainability

### 2. One role per skill
Each skill should focus on one kind of work.

This avoids:
- overlap
- trigger ambiguity
- role confusion
- bloated instructions

### 3. Document-first workflow
Whenever possible, earlier-stage skills should create documents that later-stage skills can follow.

This reduces silent assumption drift between planning, design, architecture, implementation, and testing.

### 4. Explicit handoffs
Each skill should leave behind outputs that make the next skill easier to execute.

### 5. Orchestration is control, not production
`workflow-orchestrator` should manage flow, not replace the specialized skills.

---

## Repository Structure

A typical structure looks like this:

```text
WA-SKILLS/
├── AGENTS.md
├── README.md
├── project-manager/
│   └── SKILL.md
├── uiux-designer/
│   └── SKILL.md
├── repo-architect/
│   └── SKILL.md
├── backend-implementer/
│   └── SKILL.md
├── frontend-implementer/
│   └── SKILL.md
├── test-engineer/
│   └── SKILL.md
├── deploy-readiness-check/
│   └── SKILL.md
└── workflow-orchestrator/
    └── SKILL.md