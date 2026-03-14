---
name: backend-implementer
description: Implement backend logic, APIs, database changes, middleware, and server-side integrations according to approved requirements and architecture. Use this skill for backend execution, not for redefining scope, leading UI decisions, or deployment review.
---

# Purpose

This skill implements server-side functionality based on approved planning and architecture.

Use this skill to:
- implement API endpoints
- implement service logic
- implement data access
- implement models and migrations
- implement middleware
- implement auth/session/server integrations
- align backend behavior with documented contracts

Do not use this skill to:
- redefine product scope
- decide UI direction
- replace architecture planning
- run deployment readiness reviews

# When this skill should trigger

Trigger this skill when the task involves:
- backend code
- API implementation
- database schema changes
- migrations
- service layer logic
- repositories or ORM usage
- middleware
- auth/session implementation
- server-side validation
- integrations from the backend side

# When this skill should NOT trigger

Do not trigger this skill when the task is mainly about:
- requirement discovery
- UI style questions
- frontend rendering or component styling
- test ownership as the main task
- release or deploy checklist review

# Inputs expected

This skill expects:
- project scope
- acceptance criteria
- architecture docs
- API contract
- data model direction
- relevant repo conventions

If required documents are missing, explicitly state what is missing before implementation.

# Outputs

Primary outputs:
- backend code changes
- database migrations if needed
- API wiring
- server validation logic
- focused implementation notes in markdown if needed

Optional file:
- `docs/backend-implementation-notes.md`

# Implementation rules

- Follow existing repo conventions.
- Do not silently change approved contracts unless the change is required and clearly documented.
- Keep edits bounded to backend concerns.
- Prefer incremental, reviewable changes.
- Preserve compatibility where reasonable.
- Add comments or notes only when they increase clarity.

# Constraints and non-goals

Non-goals:
- redesigning the architecture
- redesigning the frontend
- replacing the test engineer
- making deploy decisions

Constraints:
- respect documented API contracts
- keep implementation aligned with acceptance criteria
- call out blockers instead of inventing hidden product behavior
- avoid mixing unrelated refactors into feature work

# Workflow

1. Read the relevant project and architecture docs.
2. Locate affected backend files and patterns.
3. Map the requested feature to models, services, endpoints, and middleware.
4. Implement the smallest coherent backend slice.
5. Update or add migrations if needed.
6. Run available backend validation checks where appropriate.
7. Document any contract deviation or unresolved blockers.
8. Summarize changed files and expected frontend impact.

# Handoff expectations

Hand off to:
- `frontend-implementer` when frontend integration depends on completed backend behavior
- `test-engineer` for validation and regression coverage

Your handoff must clearly state:
- changed endpoints
- changed request/response behavior
- changed data assumptions
- migration requirements
- mock data or setup notes needed for testing