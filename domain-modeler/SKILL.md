---
name: domain-modeler
description: Clarify bounded contexts, ubiquitous language, aggregates, invariants, and context relationships for business-complex web-project features before repository architecture or implementation.
---

# Domain Modeler

## What It Does

- Clarifies the business model for a scoped feature or domain area before structure or implementation hardens the wrong concepts.
- Defines bounded contexts, shared language, aggregates, invariants, and important lifecycle rules.
- Produces model guidance that `repo-architect` and implementation skills can consume without inventing domain semantics ad hoc.

## When It Should Trigger

- Business terminology is unstable, ambiguous, or inconsistent across the request or existing code.
- A change needs aggregate boundaries, lifecycle rules, or domain ownership defined first.
- Cross-module or cross-system behavior is hard to explain without clearer domain concepts or context boundaries.

## When It Should Not Trigger

- The task is mainly repository layout, module foldering, or interface seams; use `repo-architect`.
- The work is routine implementation against an already stable domain model.
- The request is mainly product scoping, visual design, or deployment review.

## Expected Inputs

- Scoped feature or project brief
- Existing terminology, business rules, or behavior examples
- Known integrations, external models, or organizational boundaries
- Relevant code or docs when the current implementation already exposes domain confusion

## Expected Outputs

- `docs/domain-model.md`
- Defined bounded contexts or a justified single-context decision
- Ubiquitous language for the scoped area
- Aggregate, invariant, and lifecycle guidance where relevant
- Handoff notes for `repo-architect`, `backend-implementer`, `frontend-implementer`, and `test-engineer`

## Workflow

### 1. Frame The Domain Problem

- Restate the business capability being modeled.
- Separate product scope from domain semantics.
- Identify where current language or ownership is ambiguous enough to block safe design.

### 2. Normalize The Language

- Choose the terms that code, tests, and documents should share.
- Call out overloaded or misleading terms explicitly.
- Prefer one clear term per concept inside the current context.

### 3. Define Context Boundaries

- Decide whether the scoped work fits one bounded context or multiple.
- Identify where translation, anti-corruption, or explicit boundary ownership is needed.
- Keep context decisions concrete enough for implementers to follow.

### 4. Define Core Model Rules

- Identify entities, value objects, services, or other model concepts only when they clarify real business behavior.
- Define aggregate boundaries, invariants, and lifecycle transitions where they materially affect implementation.
- Keep persistence shape and framework constraints out of the model unless they change business meaning.

### 5. Prepare Structural Handoff

- Hand repository and interface consequences to `repo-architect`.
- Hand domain behavior expectations to implementation skills.
- Hand invariant and translation checks to `test-engineer`.

## Output Shape

When producing a domain-model artifact, prefer this structure:

```md
# Domain Model Brief

## Objective
- Domain area being clarified

## Ubiquitous Language
- Preferred terms
- Terms to avoid or translate

## Bounded Contexts
- Contexts and ownership lines

## Core Model
- Key concepts
- Aggregates or lifecycle boundaries
- Important invariants

## Integration Boundaries
- External models or translations

## Handoff Notes
- What `repo-architect` should preserve
- What implementers and tests should preserve
```

## Handoff Expectations

### To `repo-architect`

Provide:

- bounded contexts or the decision to stay within one context
- ownership lines that affect modules, APIs, or adapters
- model constraints that structure must preserve

Do not provide:

- ad hoc folder trees
- framework-driven architecture presented as domain truth

### To Implementation Skills

Provide:

- the domain language to use in code and tests
- invariants, lifecycle rules, and boundary translations
- the concepts that should own business behavior

Do not provide:

- persistence-first shortcuts that redefine the model accidentally
- generic DDD ceremony without a concrete business reason

### To `test-engineer`

Provide:

- invariants, valid/invalid transitions, and translation boundaries worth validating
- the terms tests should use so they read like executable domain examples

## Non-Goals

- Repository foldering or interface seam design
- Generic product discovery from scratch
- Production code implementation
- Broad enterprise-pattern cataloguing without a concrete domain problem
