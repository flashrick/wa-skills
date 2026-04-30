# WA-SKILLS

A reusable **Agent Skills** library for **Codex CLI** web-project workflows and tightly scoped sidecar tasks.

This repository contains a set of narrow, role-based skills designed to help structure and execute a web project through staged collaboration.

The skills are intentionally separated by responsibility so they can be invoked explicitly or discovered more reliably through their metadata.

---

## Purpose

This repository is a **skills library**, not a product application.

It is designed to support a staged web-development workflow where different skills handle different types of work, such as:

- requirement clarification
- UI/UX design
- technical architecture
- project-level agent operating rules
- persistent execution planning across sessions
- short-lived execution plans for specific coding tasks
- codebase reconnaissance before risky changes
- large migration and multi-file refactor planning
- bug root-cause investigation
- application security review for web risks
- feature impact and regression analysis
- backend implementation
- frontend implementation
- engineering code review
- testing and validation
- CI failure triage
- PR feedback resolution
- deployment readiness
- workflow orchestration
- colloquial Chinese to natural English translation when context-aware language adaptation is needed

The goal is to keep each skill:
- narrow
- reusable
- practical
- easy to trigger correctly
- easy to hand off to the next stage

Most skills here support web-project execution directly. A small number of sidecar skills may support adjacent communication work when their scope is equally narrow and reusable.

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

### 4. `agents-md-maintainer`
Responsible for:
- creating a project-root `AGENTS.md`
- updating stale or generic `AGENTS.md` rules
- documenting repo-specific commands, boundaries, and workflow guardrails for Codex
- auditing whether repository instructions match the actual codebase
- realigning `AGENTS.md` after implementation changes alter repo operating rules

Typical outputs:
- `AGENTS.md`

Use this skill when:
- a project repository does not yet have `AGENTS.md`
- Codex needs repo-specific operating instructions
- existing agent guidance is outdated, vague, or contradictory
- repository evolution made existing Codex operating rules stale

Do not use this skill for:
- product scoping
- UI or architecture design
- mainline feature implementation
- release-readiness analysis
- ordinary `README.md` maintenance that does not change Codex operating rules

---

### 5. `plans-md-maintainer`
Responsible for:
- creating a project-root `PLANS.md`
- keeping multi-step project status, milestones, blockers, and next actions current across sessions
- persisting accepted decisions and open questions after meaningful progress or routing changes

Typical outputs:
- `PLANS.md`

Use this skill when:
- the work spans multiple stages or sessions
- the project needs a durable execution plan artifact
- an existing `PLANS.md` is stale and needs to reflect current reality

Do not use this skill for:
- initial scope clarification before the work is real enough to sequence
- real-time routing without updating a durable plan
- feature implementation or testing work

---

### 6. `task-plan-generator`
Responsible for:
- creating short-lived execution plans for specific coding tasks
- defining in-scope and out-of-scope work
- ordering discovery, edits, validation, and handoff steps
- recording risks and open questions without creating durable project state

Typical outputs:
- Markdown task plans, often under `plan/`

Use this skill when:
- the user asks for a coding plan before implementation
- a concrete task needs an ordered action checklist
- repository rules require a task plan artifact

Do not use this skill for:
- product requirement scoping
- durable `PLANS.md` maintenance
- broad workflow routing without a plan artifact

---

### 7. `codebase-recon`
Responsible for:
- read-first codebase orientation
- mapping entry points, tests, contracts, and dependency signals
- using local repository evidence such as `git`, `rg`, docs, and structure
- identifying high-risk files or areas to read before changes

Typical outputs:
- codebase reconnaissance summary
- reading priority map
- validation map

Use this skill when:
- a code area is unfamiliar or risky
- implementation, review, migration, or debugging needs current-state orientation
- local evidence should guide what to read first

Do not use this skill for:
- future architecture design
- direct implementation
- focused regression review for a known change
- concrete bug root-cause diagnosis

---

### 8. `codebase-migration-planner`
Responsible for:
- planning large migrations, dependency upgrades, framework upgrades, API renames, and multi-file refactors
- defining exact transforms and blast radius
- batching work into reviewable slices
- planning codemod strategy, validation gates, and rollback

Typical outputs:
- `docs/migration-plan.md`

Use this skill when:
- a broad mechanical or structural change needs planning before edits
- a migration must be split into safe batches
- compatibility and rollback need to be explicit

Do not use this skill for:
- ordinary feature architecture
- small direct implementations
- executing all migration batches by default

---

### 9. `bug-investigator`
Responsible for:
- reproducing reported bugs or failing behavior
- narrowing root cause with evidence
- mapping symptoms to code paths, data conditions, logs, or recent changes
- preparing a fix route and verification targets

Typical outputs:
- bug investigation summary
- reproduction notes
- root-cause or hypothesis report

Use this skill when:
- the cause of a bug is unclear
- a failing behavior needs diagnosis before implementation
- logs, screenshots, or failing tests need to be traced to code

Do not use this skill for:
- obvious fixes that can go straight to implementation
- CI provider or remote runner failures
- general code review
- test writing

---

### 10. `feature-impact-reviewer`
Responsible for:
- reviewing what an intended or completed feature change could break
- identifying shared contracts, dependent behavior, and regression-sensitive edges
- suggesting the most valuable verification targets before or after implementation

Typical outputs:
- `docs/feature-impact.md`

Use this skill when:
- a change modifies existing behavior rather than adding an isolated new screen or endpoint
- the user asks what could break
- a quick regression-oriented review is needed before or after implementation

Do not use this skill for:
- full-repo audits
- broad architecture design
- primary implementation work
- owning test execution

---

### 11. `web-security-reviewer`
Responsible for:
- reviewing scoped web features or changed areas for application security risk
- identifying auth, permission, tenant-isolation, input-handling, data-exposure, and browser-security issues
- recommending focused mitigations and security verification targets

Typical outputs:
- `docs/security-review.md`
- `docs/security-checklist.md`

Use this skill when:
- the work touches login, session, roles, permissions, uploads, rich text, webhooks, CORS, cookies, JWTs, redirects, or sensitive data
- the user asks for a security review
- a release candidate needs focused app-security scrutiny

Do not use this skill for:
- full infrastructure or network security design
- formal penetration testing
- primary feature implementation
- generic code review unrelated to security risk

---

### 12. `backend-implementer`
Responsible for:
- backend code
- APIs
- database models and migrations
- service logic
- middleware
- server-side integrations
- targeted `README.md` sync when implementation changes documented usage or behavior

Typical outputs:
- backend code changes
- migrations
- targeted `README.md` updates when needed
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
- `AGENTS.md` maintenance unless repo operating rules changed and require `agents-md-maintainer`

---

### 13. `frontend-implementer`
Responsible for:
- pages
- components
- routes
- client-side state
- forms and interactions
- frontend API integration
- targeted `README.md` sync when implementation changes documented usage or behavior

Typical outputs:
- frontend code changes
- page and component implementation
- targeted `README.md` updates when needed
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
- `AGENTS.md` maintenance unless repo operating rules changed and require `agents-md-maintainer`

---

### 14. `code-reviewer`
Responsible for:
- reviewing scoped code changes, branches, PRs, and patches
- identifying correctness, maintainability, contract, lifecycle, and error-handling issues
- prioritizing findings by severity
- identifying missing validation tied to concrete risk

Typical outputs:
- code review findings
- open questions and test gaps
- handoff notes to implementers or specialist reviewers

Use this skill when:
- the user asks for code review, PR review, branch review, or patch review
- implementation is ready for an engineering quality pass
- a diff needs correctness and maintainability scrutiny

Do not use this skill for:
- security-only review
- regression-impact-only review
- primary implementation
- test execution

---

### 15. `test-engineer`
Responsible for:
- adding or updating tests
- running validation checks
- triaging failures
- identifying regressions
- performing browser-level interaction checks for frontend regression risks when needed
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
- CI provider or remote runner failure triage
- PR review comment resolution

---

### 16. `ci-failure-triager`
Responsible for:
- triaging failing CI checks, remote build jobs, and PR check failures
- collecting high-signal logs
- classifying failures as code, test, dependency, environment, flaky, workflow, or external-service issues
- routing the fix and defining recheck steps

Typical outputs:
- `docs/ci-failure-report.md` when useful
- CI failure triage summary
- recheck plan

Use this skill when:
- CI, GitHub Actions, or another remote check is red
- a local pass differs from a CI failure
- logs must be classified before a fix is chosen

Do not use this skill for:
- ordinary test writing
- local bug diagnosis after reproduction
- deployment readiness review

---

### 17. `pr-feedback-resolver`
Responsible for:
- inventorying pull request review comments or issue feedback
- grouping duplicate or related comments
- turning selected feedback into scoped fixes
- drafting concise replies and tracking unresolved items

Typical outputs:
- PR feedback inventory
- resolution plan
- reply drafts
- verification summary

Use this skill when:
- the user wants to address PR comments, requested changes, or review feedback
- comments are pasted or available from a PR tool
- feedback needs to be grouped, selected, implemented, and replied to

Do not use this skill for:
- fresh code review without supplied feedback
- CI failure triage
- broad refactors beyond review comments

---

### 18. `deploy-readiness-check`
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

### 19. `workflow-orchestrator`
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

### 20. `spoken-cn-to-natural-en-translator`
Responsible for:
- translating colloquial Chinese into natural English
- adapting wording to English-first syntax and tone
- inferring the correct technical or contextual term when Chinese wording is informal or imprecise
- preserving intent instead of mirroring Chinese phrasing mechanically

Typical outputs:
- natural English translation
- brief terminology or ambiguity notes only when needed

Use this skill when:
- spoken or chat-style Chinese should be translated into fluent English
- literal translation would sound unnatural or misleading
- domain terms or proper nouns need contextual normalization

Do not use this skill for:
- literal study-oriented translation
- writing original English without Chinese source text
- long-form English rewriting after translation is already settled

---

## Recommended Stage Order

Default web-project workflow:

1. `project-manager`
2. `uiux-designer`
3. `repo-architect`
4. `codebase-recon` when the target code area is unfamiliar or risky
5. `task-plan-generator` when a short-lived execution plan is required
6. `agents-md-maintainer` when the repository needs a durable Codex operating contract before implementation scales
7. `plans-md-maintainer` when the work should persist across sessions as a shared execution plan
8. `backend-implementer`
9. `frontend-implementer`
10. `code-reviewer` when a completed or in-progress diff needs engineering review
11. `test-engineer`
12. `deploy-readiness-check`

After implementation, revisit:
- `backend-implementer` or `frontend-implementer` for targeted `README.md` sync when documented behavior or usage changed
- `agents-md-maintainer` only when repo operating rules for Codex became stale
- `code-reviewer` when correctness, maintainability, contracts, or missing validation need a focused review
- `pr-feedback-resolver` when review comments or requested changes need to be closed out

`web-security-reviewer` is optional but recommended when work touches auth, permissions, sensitive data, external input, tenant boundaries, or release-critical security surfaces.

`feature-impact-reviewer` is optional and fits around behavior-changing work:
- before implementation to map regression risk
- after implementation to check what may still be missing

`bug-investigator` is optional and fits before implementation when a reported bug or failing behavior has no clear root cause.

`ci-failure-triager` is optional and fits after local or remote checks fail, especially when CI behavior differs from local behavior.

`codebase-migration-planner` is optional and fits before broad upgrades, API renames, dependency migrations, or multi-file refactors.

`workflow-orchestrator` exists across the whole process and should be used to:
- determine the next step
- verify whether a stage is complete
- prevent premature execution
- maintain handoff discipline

`spoken-cn-to-natural-en-translator` is a sidecar language skill. It is used on demand when Chinese source text must become idiomatic English, and it is not part of the default web-project stage order.

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
- `$agents-md-maintainer` for project-root Codex operating rules
- `$agents-md-maintainer` again after repo-shape or workflow changes make `AGENTS.md` stale
- `$plans-md-maintainer` for project-root execution plan maintenance
- `$task-plan-generator` for a short-lived coding task plan
- `$codebase-recon` for read-first codebase orientation
- `$codebase-migration-planner` for large migrations and multi-file refactor planning
- `$bug-investigator` for root-cause diagnosis before implementation
- `$feature-impact-reviewer` for change-specific regression impact analysis
- `$web-security-reviewer` for scoped application-security review
- `$backend-implementer` for server-side implementation and targeted `README.md` sync when backend-facing usage changes
- `$frontend-implementer` for UI implementation and targeted `README.md` sync when frontend-facing usage changes
- `$code-reviewer` for scoped engineering code review
- `$test-engineer` for validation
- `$ci-failure-triager` for failing CI or remote PR checks
- `$pr-feedback-resolver` for addressing PR review comments or requested changes
- `$deploy-readiness-check` for release preparation
- `$workflow-orchestrator` for gating and routing
- `$spoken-cn-to-natural-en-translator` for colloquial Chinese to idiomatic English translation

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
├── agents-md-maintainer/
│   └── SKILL.md
├── plans-md-maintainer/
│   └── SKILL.md
├── task-plan-generator/
│   └── SKILL.md
├── codebase-recon/
│   └── SKILL.md
├── codebase-migration-planner/
│   └── SKILL.md
├── bug-investigator/
│   └── SKILL.md
├── feature-impact-reviewer/
│   └── SKILL.md
├── web-security-reviewer/
│   └── SKILL.md
├── backend-implementer/
│   └── SKILL.md
├── frontend-implementer/
│   └── SKILL.md
├── code-reviewer/
│   └── SKILL.md
├── test-engineer/
│   └── SKILL.md
├── ci-failure-triager/
│   └── SKILL.md
├── pr-feedback-resolver/
│   └── SKILL.md
├── deploy-readiness-check/
│   └── SKILL.md
├── spoken-cn-to-natural-en-translator/
│   └── SKILL.md
└── workflow-orchestrator/
    └── SKILL.md
```
