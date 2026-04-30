---
name: frontend-implementer
description: Implement client-side web feature work such as screens, components, state wiring, and browser-side interactions.
---

# Frontend Implementer

## What It Does

- Implements the browser-facing side of a scoped web feature.
- Builds or updates screens, components, routes, state wiring, and client-side interactions.
- Produces frontend changes that follow the agreed UX guidance, contracts, and architecture.

## When It Should Trigger

- The task is to add or modify client-side behavior for a web feature.
- A feature has enough scope and UX definition to begin implementation.

## When It Should Not Trigger

- The task is mainly about requirement clarification, design discovery, backend implementation, test-only work, or deploy review.
- UX decisions or contracts are still too unclear for safe implementation.
- The request is really about visual direction rather than building the UI.

## Expected Inputs

- Scoped feature brief and acceptance criteria
- UX guidance for flows, screens, states, and interactions
- Architecture guidance when module boundaries matter
- Known backend or platform contracts
- Existing frontend conventions and component patterns

## Expected Outputs

- Frontend code changes for the targeted feature slice
- Any necessary route, state, and client integration updates
- UI implementation that preserves the design brief, semantic tokens, responsive rules, and interaction states
- Targeted `README.md` updates when the implemented change materially affects documented setup, usage, supported screens or flows, or developer-facing behavior
- Focused notes on UX deviations, blockers, or contract assumptions
- Explicit handoff notes for `test-engineer`

## Workflow

### 1. Confirm Readiness To Implement

- Restate the client-visible outcome being built.
- Identify the affected pages, components, routes, forms, and state transitions.
- If UX or contract gaps would make implementation unsafe, surface them explicitly instead of inventing hidden behavior.

### 2. Inspect Existing Patterns

- Read the current frontend structure and conventions before editing.
- Find the closest existing patterns for layout, forms, state, loading, and error handling.
- Reuse established patterns unless there is a documented reason not to.

### 3. Map the Change

- Determine which screens, shared components, hooks, routes, or stores own the feature.
- Keep the slice coherent so visible behavior, state transitions, and data calls line up.
- Avoid mixing unrelated visual cleanup or refactors into the work.

### 4. Implement the Smallest Coherent Slice

- Make the minimum frontend changes required for the scoped behavior.
- Follow the approved flow, states, and component intent.
- Keep loading, empty, error, and success behavior explicit when relevant.
- Convert design guidance into maintainable implementation primitives where the repo supports it:
  - use semantic tokens or existing theme variables before adding one-off colors
  - keep typography, spacing, radius, shadows, and z-index values consistent with nearby patterns
  - use a consistent icon family and avoid emoji as structural UI icons
  - preserve visible hover, pressed, focus, active, loading, disabled, and error states
  - avoid interaction styles that resize controls, shift surrounding layout, or obscure content
  - respect reduced-motion settings when adding motion
- If the design brief includes a page-specific rule, keep it local; if the rule is reused across screens, route or implement it through the existing shared styling/component layer.

### 5. Check UI Fidelity And Browser Risk

- Compare the implementation against the UX brief before handing off.
- Verify text fits within buttons, tabs, cards, tables, sidebars, and compact panels at relevant viewport widths.
- Check that fixed or sticky UI does not hide scroll content, toasts, menus, modals, or bottom actions.
- Treat these as browser-level verification triggers:
  - overlays, dialogs, drawers, popovers, dropdowns, sticky headers, sticky footers, or z-index changes
  - responsive layout changes across mobile, tablet, and desktop
  - pointer, keyboard, focus, hover, drag, or touch interactions
  - animation or transition behavior
  - charts, canvases, media, or visual assets that must actually render
- When browser tooling is available and the feature depends on these risks, run the narrowest practical browser check or tell `test-engineer` exactly what remains unverified.

### 6. Verify The Behavior

- Run relevant frontend checks when available.
- Review the result against the acceptance criteria and UX guidance.
- Call out anything that remains unverified or any deliberate deviation.

### 7. Sync Developer-Facing Docs

- Update `README.md` when the frontend change makes current setup, usage, supported screens or flows, or developer-facing notes inaccurate.
- Keep `README.md` changes narrow and factual.
- Do not expand this into repository operating-rule maintenance; route Codex workflow or path-boundary changes to `agents-md-maintainer`.

### 8. Prepare Handoff

- Summarize what changed for `test-engineer`.
- If implementation exposed unresolved UX ambiguity, state exactly what needs to return to `uiux-designer`.

## Questioning Strategy

- Ask questions only when they change visible behavior, state transitions, or integration expectations.
- Prefer confirming an inferred flow over requesting a new design discussion.
- Push unclear design questions to `uiux-designer` and unclear structure questions to `repo-architect`.
- Do not ask the user to specify routine implementation mechanics that can be derived from the codebase.

Use questions like these when needed:

- "What is the primary user action this screen must support in the first release?"
- "Which states are required now: loading, empty, validation, error, success, or permissions?"
- "Should this match an existing screen or component pattern?"
- "Is the backend contract stable enough to wire directly, or should the UI tolerate a provisional shape?"
- "What visible behavior matters most if tradeoffs are needed in the first slice?"

Avoid questions like these unless the user explicitly asks for that depth:

- broad scope questions owned by `project-manager`
- visual exploration questions owned by `uiux-designer`
- backend contract or data-layer design questions owned by `repo-architect` or `backend-implementer`

## Output Shape

When summarizing frontend work, prefer this structure:

```md
# Frontend Implementation Notes

## Objective
- User-visible behavior added or changed

## Changed Areas
- Pages, components, routes, hooks, or stores touched

## Interaction Notes
- Key flows
- Important states
- Contract assumptions
- Design brief fidelity

## Browser Risk Notes
- Viewports or device classes affected
- Overlays, sticky elements, focus paths, animation, charts, media, or visual assets touched
- UI quality gates still needing evidence

## Validation
- Checks run
- What remains unverified

## Handoff Notes
- What `test-engineer` should verify
- What needs UX follow-up, if any
```

Keep the notes concrete:

- name the visible behavior, not just the files
- separate implemented behavior from unresolved UX issues
- flag any compromise or temporary handling explicitly

## Handoff Expectations

### To `test-engineer`

Provide:

- changed user-visible behaviors
- states and edge cases worth validating
- browser-risk areas such as overlays, responsive layout, focus, animation, or visual assets
- any UI quality gates from `uiux-designer` that still need evidence
- setup or navigation prerequisites
- any known gaps or temporary handling
- note whether `README.md` was updated or intentionally left unchanged because the change did not affect documented usage

Do not provide:

- a request to rediscover the feature intent
- vague "UI updated" notes without behavioral detail
- hidden design changes that were not escalated

### To `uiux-designer`

Provide only if implementation uncovered a real design gap:

- the ambiguous flow or state
- what was implemented temporarily, if anything
- what decision is needed to finish cleanly

Do not provide:

- routine implementation questions that can be solved from the codebase
- backend concerns disguised as UX feedback

## Non-Goals

- Clarifying broad product scope
- Defining visual direction from scratch
- Defining repository structure or service boundaries
- Owning backend implementation
- Acting as the sole testing or deploy-readiness workflow
