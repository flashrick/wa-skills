---
name: repo-architect
description: Define technical architecture, module boundaries, API contracts, data flow, and implementation sequencing for a web project. Use this skill for architecture and integration planning, not for requirement interviews, visual design collection, or direct feature implementation.
---

# Purpose

This skill turns approved requirements and design intent into a concrete technical plan.

Use this skill to:
- define architecture
- define module boundaries
- define API contracts
- define data model direction
- define state and data flow
- define implementation order
- reduce ambiguity between backend and frontend

Do not use this skill to:
- interview the user for general requirements
- decide visual style
- write most production code
- perform testing or deployment validation

# When this skill should trigger

Trigger this skill when the task involves:
- architecture
- folder structure
- code organization
- API contract definition
- data model planning
- client/server boundaries
- integration sequencing
- implementation strategy
- technical decomposition

# When this skill should NOT trigger

Do not trigger this skill when the task is mainly about:
- broad product requirement clarification
- UI preference discovery
- writing final frontend code
- writing final backend code
- executing tests
- deployment readiness checks

# Inputs expected

This skill expects:
- project scope
- acceptance criteria
- UI/UX documents if available
- current repo structure, if repo already exists
- known stack decisions
- constraints such as framework, hosting, auth, or database choice

# Outputs

Create or update these files when useful:
- `docs/architecture.md`
- `docs/api-contract.md`
- `docs/data-model.md`
- optionally `docs/implementation-order.md`

# Output requirements

## `docs/architecture.md`
Should include:
- high-level system overview
- frontend/backend responsibilities
- module boundaries
- key libraries or frameworks already assumed
- state management direction
- data flow summary
- auth/session assumptions
- integration points
- risky areas and simplifications

## `docs/api-contract.md`
Should include:
- route list
- method
- request shape
- response shape
- auth expectations
- error shape
- pagination/filtering rules if relevant

## `docs/data-model.md`
Should include:
- entities
- important fields
- relationships
- constraints
- lifecycle notes
- migration or schema notes if relevant

## `docs/implementation-order.md` (optional)
Should include:
- recommended phase order
- dependency notes
- parallelizable tasks
- blocked tasks

# Constraints and non-goals

Non-goals:
- replacing the project manager
- replacing UI design work
- rewriting requirements
- writing large implementation patches unless explicitly asked

Constraints:
- prefer simple architecture that fits the repo
- prioritize clear handoffs to backend and frontend implementers
- avoid speculative complexity
- document assumptions explicitly

# Workflow

1. Read project scope and acceptance criteria.
2. Read UI/UX docs if they exist.
3. Inspect the repo if one exists.
4. Identify required modules and boundaries.
5. Define API contracts and data model direction.
6. Define implementation sequence and dependency graph.
7. Write architecture documents.
8. Summarize what backend and frontend implementers should follow strictly.

# Handoff expectations

Hand off to:
- `backend-implementer`
- `frontend-implementer`

Your handoff must clearly state:
- fixed contracts
- module boundaries
- required order of work
- what can be parallelized
- assumptions that must not be silently changed