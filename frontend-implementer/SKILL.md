---
name: frontend-implementer
description: Implement frontend pages, components, routes, client-side state, and API integration according to approved UI/UX guidance and architecture. Use this skill for frontend execution, not for collecting design preferences, redefining requirements, or backend implementation.
---

# Purpose

This skill implements the client-side product experience using approved design and architecture documents.

Use this skill to:
- build pages
- build components
- implement routing
- implement forms and interactions
- integrate client-side state
- call approved APIs
- connect the UI to documented backend behavior

Do not use this skill to:
- lead product requirement clarification
- lead visual design discovery
- redesign architecture
- implement backend systems

# When this skill should trigger

Trigger this skill when the task involves:
- frontend page implementation
- component implementation
- route wiring
- forms and interactions
- client-side state
- loading/error/empty states
- responsive implementation
- API consumption on the client side

# When this skill should NOT trigger

Do not trigger this skill when the task is mainly about:
- collecting visual preferences
- deciding the overall design language
- backend endpoints or database logic
- full testing ownership
- deployment readiness

# Inputs expected

This skill expects:
- project scope
- acceptance criteria
- UI style guide
- page structure docs
- component spec
- architecture docs
- API contract

# Outputs

Primary outputs:
- frontend code changes
- component implementations
- page implementations
- route updates
- state management integration
- API client integration

Optional file:
- `docs/frontend-implementation-notes.md`

# Implementation rules

- Follow the approved design system.
- Follow documented API contracts.
- Keep behavior and appearance consistent across pages.
- Implement loading, empty, and error states where relevant.
- Avoid hidden redesigns during implementation.
- Prefer reusable components over one-off duplication.

# Constraints and non-goals

Non-goals:
- redefining UI strategy from scratch
- inventing backend contracts
- changing project scope
- acting as the primary tester or deploy checker

Constraints:
- respect UI/UX documents
- respect architecture and repo conventions
- keep implementation reviewable
- document any required deviation from design or contract

# Workflow

1. Read the relevant planning, design, and architecture documents.
2. Identify affected pages, components, and routes.
3. Implement shared components first where useful.
4. Implement page structure and interactions.
5. Integrate state and API calls.
6. Add loading/error/empty states.
7. Run available frontend validation checks where appropriate.
8. Summarize changed files and any remaining UX or API blockers.

# Handoff expectations

Hand off to:
- `test-engineer`
- optionally `uiux-designer` if implementation exposed a design ambiguity

Your handoff must clearly state:
- implemented pages/components
- integrated endpoints
- unimplemented edge cases
- known UI compromises
- areas needing validation