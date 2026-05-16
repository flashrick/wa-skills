---
name: repo-architect
description: Define repository layout, module boundaries, ownership lines, and interface seams for a web project codebase once core domain semantics are stable enough to structure safely.
---

# Repo Architect

## What It Does

- Turns scoped feature requirements into a concrete codebase shape.
- Consumes upstream domain-model guidance when business semantics materially affect structure.
- Defines repository layout, module boundaries, ownership lines, and interface seams.
- Produces architecture artifacts that implementation skills can follow without inventing structure ad hoc.

## When It Should Trigger

- A web project or major feature needs repository structure, boundary decisions, or interface planning before implementation.
- Backend and frontend work would drift without clear ownership and seams.
- The domain model is already stable enough, or the work is simple enough, that structural design can proceed safely.

## When It Should Not Trigger

- The task is mainly about product scoping, UI design, direct coding, test execution, or deploy review.
- The repo structure is already stable enough and the work is routine implementation.
- The request is really about endpoint behavior or UI flow rather than codebase boundaries.
- Core domain language, aggregate boundaries, or context relationships are still the real blocker; use `domain-modeler`.

## Expected Inputs

- Scoped feature or project brief
- Acceptance criteria
- Domain-model guidance when business-complex features are involved
- UX guidance if user-facing flows already exist
- Existing repository context if any
- Known stack assumptions and integration constraints

## Expected Outputs

- An architecture brief covering repository layout and ownership
- Defined interface seams between layers or services
- A dependency-aware implementation order for backend and frontend work
- Explicit handoff notes for `backend-implementer` and `frontend-implementer`
- A redirect to `domain-modeler` when unresolved business semantics would make the structure premature

## Workflow

### 1. Frame the Architectural Problem

- Restate what the codebase must support.
- Identify whether this is greenfield structure, feature expansion, refactor boundary work, or integration layering.
- Separate fixed constraints from open architectural decisions.
- Make explicit whether domain semantics are already settled enough to structure safely.

### 2. Inspect the Existing Shape

- Read the current repo if it exists.
- Identify existing module boundaries, conventions, and pain points.
- Reuse stable patterns when possible instead of inventing a parallel structure.

### 3. Define the Boundaries

- Decide which modules, packages, services, or layers own which responsibilities.
- Clarify what crosses boundaries: APIs, events, shared types, adapters, or data contracts.
- Keep boundaries understandable to implementers, not just theoretically clean.
- Preserve upstream domain-model ownership instead of letting persistence or transport details redefine it.

### 4. Define the Interface Seams

- Document how backend, frontend, shared code, and integrations should connect.
- Call out where contracts are fixed, provisional, or intentionally deferred.
- Keep interface definitions implementation-ready but not implementation-specific.

### 5. Sequence the Work

- Recommend an implementation order that respects dependencies.
- Note what can proceed in parallel and what must be blocked.
- Flag structural decisions that must not change silently during implementation.

### 6. Prepare Handoffs

- Hand backend-facing structure and contracts to `backend-implementer`.
- Hand frontend-facing structure and contracts to `frontend-implementer`.
- If business language, aggregates, invariants, or context boundaries are still unstable, redirect to `domain-modeler` before locking structure.
- If the real gap is still in user flow or page behavior, redirect to `uiux-designer` instead of forcing architecture prematurely.

## Questioning Strategy

- Ask only the questions that materially change boundaries, ownership, or sequencing.
- Start from the existing repo and stated constraints before asking for preferences.
- Prefer confirming a proposed structure over asking open-ended architecture brainstorming questions.
- Treat unclear product scope as a blocker to raise back to `project-manager`, not a reason to broaden this skill.
- Treat unclear domain language or domain ownership as a blocker to raise to `domain-modeler`, not a reason to force architecture from technical shape alone.

Use questions like these when needed:

- "Does this need to fit an existing monolith, package structure, or service boundary?"
- "Which integrations or domains must remain isolated?"
- "Are backend and frontend expected to move independently or in the same repo slice?"
- "Is there a fixed stack choice that constrains foldering or shared contracts?"
- "What part of the structure must be stable for parallel implementation?"

Avoid questions like these unless the user explicitly asks for that depth:

- broad feature discovery questions owned by `project-manager`
- domain-language and aggregate-definition questions owned by `domain-modeler`
- visual or flow design questions owned by `uiux-designer`
- endpoint implementation details owned by implementation skills

## Output Document Shape

When producing an architecture artifact, prefer this structure:

```md
# Architecture Brief

## Objective
- What the codebase must support
- Key constraints

## Repository Layout
- Major directories, packages, or services
- Responsibility of each

## Boundaries And Interfaces
- Ownership lines
- Cross-boundary contracts
- Shared code rules

## Implementation Order
- First slice
- Dependent slices
- Parallelizable work

## Handoff Notes
- What `backend-implementer` should follow
- What `frontend-implementer` should follow
- Open architectural questions
```

Make the document reusable:

- Prefer concrete ownership and seam definitions over generic architecture prose.
- Keep provisional decisions labeled as provisional.
- Make dependency order explicit so implementation skills do not guess.

## Handoff Expectations

### To `backend-implementer`

Provide:

- backend-owned modules or services
- interface seams and data boundaries
- domain-model constraints that structure must preserve
- dependency order and blocked areas

Do not provide:

- feature scope rewrites
- UI flow definitions
- broad implementation tasks without structural context

### To `frontend-implementer`

Provide:

- frontend-owned modules or areas
- shared contract expectations
- integration boundaries with backend or platform code
- any domain-model constraints that affect client concepts or shared types
- any sequencing constraints that affect client work

Do not provide:

- visual design decisions
- backend implementation details disguised as architecture
- speculative abstractions without delivery value

## Non-Goals

- Clarifying product scope from scratch
- Designing UI flows or visual systems
- Writing production backend or frontend code
- Owning test execution or deploy assessment
- Acting as a generic technical strategist beyond repository and boundary definition
