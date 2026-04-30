---
name: uiux-designer
description: Define web UI flows, interaction states, layout intent, and visual direction for a specific user-facing feature.
---

# UIUX Designer

## What It Does

- Turns a scoped feature into concrete UX and UI guidance for implementation.
- Defines user flows, screen intent, states, interaction behavior, and visual direction.
- Produces design artifacts that frontend work can follow without reinterpreting the feature from scratch.

## When It Should Trigger

- A user-facing web feature needs UI flow design, screen structure, interaction states, or visual direction.
- Frontend implementation is blocked on unclear UX decisions.

## When It Should Not Trigger

- The task is mainly about product scoping, repo structure, backend logic, testing, or deploy review.
- The feature is already fully designed and the next step is implementation or validation.
- The request is about generic brand strategy rather than a specific web feature.

## Expected Inputs

- Feature goal and target users
- Scope and acceptance criteria
- Existing brand cues or design constraints if any
- Known platform expectations such as desktop-first, mobile-first, or responsive
- Existing UI patterns if the product already exists

## Expected Outputs

- A feature design brief with flow, screens, states, and interaction notes
- Visual direction guidance tied to the feature
- A lightweight design-system brief when the feature needs reusable visual rules
- UI quality gates covering accessibility, interaction, responsive behavior, and visual consistency
- A reusable component and state inventory for implementation
- Explicit handoff notes for `frontend-implementer`

## Workflow

### 1. Frame the Feature

- Restate the user-facing outcome in one or two sentences.
- Identify the primary user, entry point, and success path.
- Separate confirmed requirements from assumptions.

### 2. Find the UX Gaps

- Identify what is still undefined: flow order, page structure, validation behavior, empty states, loading states, permissions, edge cases, or responsive behavior.
- Ask only the questions that materially change the flow or state design.
- If inputs are incomplete, continue with labeled assumptions instead of stalling.

### 3. Define the Core Flow

- Write the primary happy path first.
- Cover important alternate paths only when they affect real implementation or acceptance criteria.
- Keep flows tied to user goals, not abstract page lists.

### 4. Define Screens and States

- Describe each screen or major area by purpose, priority content, and key actions.
- Call out loading, empty, error, success, and permission states where relevant.
- Note responsive differences only when they change structure or behavior.

### 5. Set Visual Direction

- Define tone, density, hierarchy, and interaction feel at a practical level.
- Reuse existing product patterns when they exist.
- Avoid drifting into a full design system unless the feature truly needs it.
- When a feature needs a reusable visual baseline, produce a lightweight design-system brief before implementation:
  - product or workflow type and target audience
  - page or flow pattern, such as data-dense dashboard, trust-building onboarding, editorial content, or conversion-focused landing
  - color role guidance using semantic roles, not only raw hex values
  - typography scale and density expectations
  - spacing rhythm, responsive breakpoints, and layout constraints
  - motion, feedback, and state principles
  - anti-patterns that would hurt this product type
- Use a domain decision matrix when the design is under-specified:
  - product: user intent, trust level, data density, and expected workflow pace
  - style: visual language that fits the product type and avoids trend mismatch
  - color: semantic roles, contrast needs, brand constraints, and theme behavior
  - typography: hierarchy, reading density, label legibility, and platform fit
  - layout: page pattern, navigation model, responsive structure, and content priority
  - interaction: feedback timing, focus behavior, gestures, and state transitions
  - data visualization: chart type, legend, tooltip, empty state, and color-not-only requirements

### 6. Define UI Quality Gates

- Set concrete acceptance checks for the UI direction before handing off.
- Include only checks that matter to the scoped feature; do not paste a generic checklist.
- Consider these categories:
  - accessibility: contrast, keyboard path, labels for icon-only controls, heading order, reduced motion
  - interaction: visible hover/pressed/focus/disabled states, predictable feedback, no layout-shifting state changes
  - responsive layout: small mobile, tablet, desktop, no horizontal scroll, text does not overflow controls
  - visual consistency: semantic tokens, icon family consistency, spacing rhythm, clear hierarchy
  - content states: loading, empty, error, success, permissions, and destructive-confirmation states
- Mark any gate that needs browser validation so `test-engineer` can verify it later.

### 7. Prepare Handoff

- Package the flow, screen guidance, component inventory, and unresolved questions for `frontend-implementer`.
- If implementation reveals a structural repo concern rather than a design issue, route that question to `repo-architect`.

## Questioning Strategy

- Start from the scoped feature and ask only what changes the UX decision.
- Prefer a draft-first approach: propose a likely flow, then ask the user to confirm or correct it.
- Ask about user behavior, priority, and constraints before asking about aesthetics.
- Avoid broad taste interviews unless the request is explicitly about visual exploration.

Use questions like these when needed:

- "Who is the primary user for this flow?"
- "What must the user be able to complete without friction?"
- "Which states matter for the first release: empty, loading, error, approval, or permissions?"
- "Is there an existing product pattern this feature should match?"
- "Does mobile need different structure, or just responsive adaptation?"

Avoid questions like these unless the user explicitly asks for that depth:

- backend contract questions owned by `repo-architect` or `backend-implementer`
- feature scope questions that belong with `project-manager`
- implementation library choices that belong with implementation skills

## Output Document Shape

When producing a design artifact, prefer this structure:

```md
# Feature Design Brief

## Objective
- Feature goal
- Target users
- Success outcome

## Core Flow
- Entry point
- Main user path
- Important alternate paths

## Screens And States
### Screen or Area
- Purpose
- Key content
- Primary actions
- Loading, empty, error, and success states

## Visual Direction
- Tone and hierarchy
- Layout intent
- Interaction notes
- Responsive notes

## Design System Brief
- Pattern or layout model
- Semantic color roles
- Typography and density
- Spacing and responsive rules
- Motion and feedback rules
- Anti-patterns to avoid

## Components
- Reusable components
- Variants and state requirements

## UI Quality Gates
- Accessibility checks
- Interaction checks
- Responsive checks
- Visual consistency checks
- Browser validation needs

## Handoff Notes
- What `frontend-implementer` should follow strictly
- Open UX questions
```

Make the document reusable:

- Write in terms of user actions and visible behavior.
- Keep unresolved design questions explicit.
- Prefer concrete screen and state definitions over mood-board language.

## Handoff Expectations

### To `frontend-implementer`

Provide:

- the core flow and alternate paths
- screen purpose and priority actions
- required states and validation behavior
- reusable components and interaction rules
- design-system brief details that should become tokens, reusable styles, or component variants
- feature-specific UI quality gates and browser-validation needs
- constraints that must remain intact during implementation

Do not provide:

- backend contracts
- repo structure decisions
- vague design language without actionable screen guidance

## Non-Goals

- Clarifying broad product scope
- Defining repository layout or service boundaries
- Writing production frontend code
- Defining backend APIs or data models
- Owning full-project planning or release readiness
