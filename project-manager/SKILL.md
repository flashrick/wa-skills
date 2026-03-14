---
name: project-manager
description: Clarify web-project requirements, define scope slices, and write acceptance criteria before design or implementation starts.
---

# Project Manager

## What It Does

- Converts an early web-project request into a kickoff-ready scope definition.
- Clarifies the goal, users, constraints, delivery slices, and acceptance criteria.
- Produces planning artifacts that design, architecture, and implementation skills can reuse directly.

## When It Should Trigger

- The request is still fuzzy and needs requirement clarification, scope boundaries, task decomposition, or acceptance criteria.
- The team needs a concrete kickoff document before handing work to `uiux-designer`, `repo-architect`, or implementation skills.

## When It Should Not Trigger

- The request is primarily about UI flows, visual direction, repo structure, coding, testing, or deploy readiness.
- The feature is already scoped well enough that the next step is design, architecture, or implementation.
- The user wants ongoing coordination, staffing, status tracking, or broad project governance.

## Expected Inputs

- Product or business goal
- Target user, operator, or audience if known
- Constraints such as timeline, budget, stack, policy, or integration requirements
- Existing notes, tickets, or rough feature ideas if available

## Expected Outputs

- A kickoff brief with problem, users, scope, assumptions, constraints, and risks
- Delivery slices with dependency order and suggested milestone boundaries
- Acceptance criteria written at feature or slice level
- Explicit handoff notes for `uiux-designer` and `repo-architect` when those skills are the next step

## Workflow

### 1. Frame the Request

- Restate the requested outcome in one or two sentences.
- Identify what kind of web-project kickoff this is: new product, major feature, workflow redesign, integration, admin tooling, or migration slice.
- Separate known facts from assumptions immediately.

### 2. Find the Missing Decisions

- Look for gaps that block scoping: user type, success outcome, key workflow, constraints, external dependencies, rollout expectations, or deadline pressure.
- Ask only the questions that materially change scope or acceptance criteria.
- If the request is still incomplete after a small number of high-value questions, continue with labeled assumptions instead of stalling.

### 3. Define the Scope Boundary

- Write a clear in-scope list and out-of-scope list.
- Call out risky edge cases, future phases, and items intentionally deferred.
- Prefer smaller first-delivery slices over broad "phase 1 includes everything" plans.

### 4. Decompose the Work

- Break the effort into delivery slices that a team could actually sequence.
- Keep slices outcome-based, such as "admin can create draft campaign" or "customer can reset password with email link".
- Note dependencies between slices, but do not design implementation details.

### 5. Write Acceptance Criteria

- Attach acceptance criteria to each slice or feature area.
- Use observable behavior, business rules, and boundary conditions.
- Include operational or compliance constraints only when they change readiness or scope.

### 6. Prepare Handoffs

- If user-facing behavior is still undefined, hand off to `uiux-designer` with feature goal, target users, primary flows, states needing design, and any constraints already decided.
- If codebase structure or service boundaries are still undefined, hand off to `repo-architect` with scope slices, integration points, stack assumptions, and unresolved architectural decisions.
- Only point to implementation skills after the request has a stable enough scope, acceptance criteria, and upstream handoff outputs.

## Questioning Strategy

- Start with a draft-first mindset: infer a likely scope from the request, then ask targeted questions to confirm or correct it.
- Ask for decisions, not brainstorming. Good questions change scope, sequencing, or acceptance criteria.
- Prefer the smallest set of questions needed to remove ambiguity. Avoid long discovery questionnaires.
- Ask about one level deeper than the user already provided. Do not ask for information that is irrelevant to kickoff planning.
- Distinguish blockers from nice-to-know details.

Use questions like these when needed:

- "Who is the primary user for the first release?"
- "What outcome must be possible on day one, and what can wait?"
- "Are there non-negotiable constraints such as existing stack, auth, integrations, or compliance?"
- "Is this replacing an existing workflow or introducing a new one?"
- "What would make this release unsuccessful even if the code works?"

Avoid questions like these unless the user explicitly asks for that depth:

- Detailed UI layout or component behavior questions better handled by `uiux-designer`
- Folder structure, service boundary, or module ownership questions better handled by `repo-architect`
- Endpoint design, schema design, test implementation, or rollout mechanics owned by implementation and follow-on skills

## Output Document Shape

When producing a kickoff artifact, prefer this structure:

```md
# Kickoff Brief

## Objective
- What problem this work solves
- Who it is for
- What success looks like

## Scope
- In scope
- Out of scope
- Assumptions
- Constraints

## Delivery Slices
### Slice 1
- Outcome
- Dependencies
- Acceptance criteria

### Slice 2
- Outcome
- Dependencies
- Acceptance criteria

## Risks And Open Questions
- Key risk
- Open decision

## Handoffs
- `uiux-designer`: what needs UX definition next
- `repo-architect`: what needs structural definition next
```

Make the document reusable:

- Write it so another skill can act on it without re-deriving the basics.
- Prefer concrete nouns, user actions, and business rules over vague summaries.
- Keep unresolved items visible instead of hiding them in prose.

## Handoff Expectations

### To `uiux-designer`

Provide:

- the objective and target users
- the in-scope flows or jobs to be done
- the states or decisions that still need UX treatment
- any constraints already fixed, such as platform, brand, compliance, or device context

Do not provide:

- invented screen designs
- detailed component specs
- visual direction masquerading as scope planning

### To `repo-architect`

Provide:

- the scoped slices and dependency order
- relevant integrations, data domains, and system touchpoints
- stack assumptions or constraints already known
- unresolved structural questions that affect module boundaries or ownership

Do not provide:

- ad hoc folder trees
- interface contracts presented as settled architecture
- implementation task lists disguised as architecture

## Non-Goals

- Managing execution after kickoff
- Designing UI flows or visual systems
- Designing repository layout or module structure
- Writing production code, migrations, tests, or deployment plans
- Acting as a generic program manager for all project work
