# AGENTS.md

## Repository Purpose

This repository is a **skills library**, not a product application repository.

Its purpose is to define, maintain, and evolve a set of narrow, role-based **Agent Skills** for use with Codex CLI.  
Each skill should have a clear boundary, a stable responsibility, and practical instructions that can be reused across projects.

Do not treat this repository like a normal web app, backend service, or frontend codebase.

---

## Core Rules

### 1. One folder = one skill
Each skill must live in its own folder.

Each skill folder must represent exactly one distinct skill and must contain a required `SKILL.md`.

Do not combine multiple unrelated roles into one skill folder.

### 2. Keep skill descriptions narrow
Each skill's `name` and `description` must be narrow, specific, and non-overlapping.

Descriptions should help Codex decide when to use the skill.  
Do not write broad descriptions such as:
- "handles the whole project"
- "does all development work"
- "manages everything related to web apps"

Prefer descriptions that clearly define:
- what the skill is for
- when it should trigger
- what it should not do

### 3. Preserve role boundaries
Do not silently merge responsibilities across skills.

Examples:
- `project-manager` should not become an implementation skill
- `uiux-designer` should not become a frontend coding skill
- `workflow-orchestrator` should not become a catch-all manager
- `test-engineer` should not become the main feature implementer

If a skill needs a new responsibility, first determine whether that responsibility belongs in:
- the existing skill
- a different existing skill
- a newly created skill

### 4. Prefer instruction-only skills
Default to instruction-only skills.

Do not add scripts, tools, or extra automation files unless there is a clear and justified need.  
Most skills in this repository should work well with `SKILL.md` instructions alone.

### 5. Keep outputs concrete
Each skill should define practical outputs whenever appropriate.

Prefer file-oriented outputs such as:
- `docs/project-scope.md`
- `docs/ui-style-guide.md`
- `docs/architecture.md`
- `docs/test-report.md`

Do not write vague instructions without clear deliverables.

### 6. Preserve consistent structure
All skills should follow a consistent `SKILL.md` structure.

Whenever possible, include:
- YAML front matter
- purpose
- when it should trigger
- when it should not trigger
- expected inputs
- expected outputs
- constraints / non-goals
- workflow
- handoff expectations

Do not invent a completely different format for one skill unless there is a strong reason.

### 7. Do not broaden existing skills casually
When modifying an existing skill:
- preserve its original purpose
- preserve its narrow trigger behavior
- preserve its role boundary

Prefer improving clarity and usefulness over expanding scope.

### 8. Update README when the skill library changes
If a skill is added, removed, renamed, or materially changed, update `README.md` accordingly.

The README should remain a useful overview of:
- available skills
- their purposes
- recommended order
- how they relate to one another

---

## Expected Repository Layout

This repository is expected to follow a structure similar to:

```text
<repo-root>/
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
