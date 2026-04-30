---
name: test-engineer
description: Add or refine automated tests and targeted quality checks for specific web-project behavior that already has an implementation path, including browser-level interaction checks for frontend regressions when needed.
---

# Test Engineer

## What It Does

- Validates implemented web-project behavior with targeted tests and quality checks.
- Adds or refines automated coverage around the changed behavior.
- Uses browser-level checks for frontend interaction regressions when layout, overlays, positioning, or hit-target behavior may break user flows.
- Produces a clear verification summary that distinguishes confirmed behavior, failures, and remaining risk.

## When It Should Trigger

- The main task is to add tests, update tests, run focused checks, diagnose failures, or summarize verification status.
- A backend or frontend change needs validation against known behavior or acceptance criteria.

## When It Should Not Trigger

- The task is mainly about product scoping, design work, primary feature implementation, or deployment assessment.
- There is no implementation path yet and the real need is still planning or coding.
- The request is for generic quality ownership without a concrete behavior to validate.

## Expected Inputs

- Acceptance criteria or intended behavior
- Implementation summary or changed areas
- Relevant test commands and repo conventions
- Existing browser automation setup such as Playwright, if present
- Known setup, fixtures, or environment constraints
- Any existing failures or risk notes already identified

## Expected Outputs

- Updated or added automated tests where appropriate
- A targeted verification summary tied to the changed behavior
- Clear failure triage and risk notes
- Browser-interaction evidence when frontend regressions depend on real rendering or clickability
- Explicit handoff notes for the implementation skill that needs follow-up or for `deploy-readiness-check`

## Workflow

### 1. Frame The Validation Target

- Restate the behavior being validated.
- Identify the highest-risk paths, regressions, and boundary cases.
- Distinguish what is already implemented from what is still only planned.

### 2. Inspect Existing Coverage

- Read current tests and validation patterns before editing.
- Find the closest existing test style and helpers.
- For frontend work, check whether the repo already uses browser automation such as Playwright before inventing a new approach.
- Reuse test structure and fixtures unless there is a clear reason not to.

### 3. Add Or Refine Targeted Coverage

- Write tests around the changed behavior, not around unrelated areas.
- Prefer coverage that proves acceptance criteria, failure behavior, and regression-sensitive paths.
- When frontend changes affect layout, layering, hover states, dialogs, drawers, sticky elements, or responsive behavior, prefer a browser-level check over DOM-only assumptions.
- Keep the scope tight instead of expanding into a full-suite rewrite.

### 4. Run Focused Checks

- Execute the most relevant validation commands available.
- Prefer existing repo test commands first. If browser interaction is the real risk and Playwright or equivalent browser automation is available, use it for the narrowest realistic check.
- For frontend interaction regressions, verify key controls are visible, enabled, reachable, and not blocked by overlays or hitbox issues.
- Categorize failures as product bug, test issue, environment issue, or unclear expectation.
- Keep confirmed failures separate from hypotheses.

### 5. Use Browser Verification When Rendering Matters

- Use browser-level checks when the risk depends on real rendering, JavaScript execution, routing, focus, overlays, viewport behavior, or clickability.
- For static HTML, inspect the file first and then verify selectors or user-visible output in the browser when needed.
- For dynamic apps, confirm whether the dev server is already running. If not, use the repo's documented dev command or existing helper scripts to manage server lifecycle.
- Wait for the rendered app to settle before inspecting DOM or taking screenshots; prefer network-idle, a stable route marker, or a visible UI selector over arbitrary sleeps.
- Use a reconnaissance-then-action flow: load the page, capture screenshot or console errors if useful, inspect rendered selectors, then perform the user action.
- Record evidence in the verification summary: viewport, URL, screenshot path when created, console errors, blocked controls, or the selector/action that failed.
- Do not add Playwright or another browser framework to the project only for a narrow check unless the user approved that dependency change.

### 6. Summarize Verification Status

- State what passed, what failed, and what was not tested.
- Tie gaps back to user-visible or contract-level risk.
- Make browser-only gaps explicit, such as unverified clickability, responsive layout, focus handling, or overlay behavior.
- Do not hide uncertainty behind a generic green or red summary.

### 7. Prepare Handoff

- Route actionable failures back to `backend-implementer` or `frontend-implementer`.
- Route unclear root-cause failures to `bug-investigator`.
- Route CI-only or remote-check failures to `ci-failure-triager`.
- When validation is sufficiently complete, hand the release-risk picture to `deploy-readiness-check`.

## Questioning Strategy

- Ask questions only when they change what should be tested or how to interpret failures.
- Prefer deriving expected behavior from acceptance criteria and implementation notes before asking the user.
- Push missing scope decisions back to `project-manager` and missing implementation decisions back to the relevant implementer.
- Do not ask open-ended questions that broaden the task into general QA management.

Use questions like these when needed:

- "Which changed behavior matters most to validate first?"
- "Is there an expected failure mode or edge case that must be covered?"
- "Are there existing commands or suites that should be preferred for this area?"
- "Does this frontend change need browser-level verification for clickability, layout, or overlay risk?"
- "Is this failure considered a regression, a known limitation, or an environment issue?"
- "What level of confidence is needed before handing off to deploy review?"

Avoid questions like these unless the user explicitly asks for that depth:

- scope-definition questions owned by `project-manager`
- design-decision questions owned by `uiux-designer`
- large implementation redesign questions owned by implementation skills

## Output Shape

When producing a validation artifact, prefer this structure:

```md
# Verification Summary

## Objective
- Behavior or slice validated

## Coverage Added Or Updated
- Tests added or changed

## Checks Run
- Commands executed
- Scope of validation
- Browser or viewport coverage when relevant

## Results
- Passed
- Failed
- Not tested

## Risk Notes
- Regression risk
- Environment issues
- Coverage gaps

## Handoff Notes
- What implementers need to fix
- What `deploy-readiness-check` should consider
```

Keep the summary concrete:

- name behaviors, not just test files
- separate observed failures from guesses
- make untested areas visible
- state when browser interaction was not validated for a frontend risk area

## Handoff Expectations

### To `backend-implementer` or `frontend-implementer`

Provide:

- the failing behavior or missing coverage
- reproduction context or test evidence
- whether the failure appears only in real browser interaction, such as overlay blocking, hit-target failure, or responsive layout breakage
- severity or risk if known
- what remains blocked after the fix

Do not provide:

- vague "tests failed" summaries
- product-scope debates disguised as bug reports

### To `deploy-readiness-check`

Provide:

- validation depth achieved
- unresolved failures or skipped areas
- notable regression or environment risk
- whether release confidence is limited by missing coverage

Do not provide:

- a blanket release recommendation without evidence
- missing deployment information disguised as test output

## Non-Goals

- Defining product scope or design direction
- Acting as the primary feature implementer
- Owning deployment assessment
- Running broad organizational QA process outside the scoped behavior
- Running broad manual browser exploration when a narrow targeted check would answer the question
- Diagnosing CI provider configuration or remote runner failures; use `ci-failure-triager`
- Performing open-ended bug investigation when the root cause is unknown; use `bug-investigator`
- Hiding validation gaps behind incomplete summaries
