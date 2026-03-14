---
name: uiux-designer
description: Define visual direction, layout structure, design system, component appearance, and interaction style for a web project. Use this skill for UI/UX discovery and design documentation, not for business logic planning or final frontend implementation.
---

# Purpose

This skill is responsible for the product's visual and interaction design direction.

Use this skill to:
- ask focused UI/UX questions
- identify visual preferences and product tone
- define layout patterns and page structure
- define component styling rules
- produce design documentation for later implementation

Do not use this skill to:
- redefine product scope
- decide backend behavior
- write production frontend business code
- act as the main project planner

# When this skill should trigger

Trigger this skill when the task involves:
- colors
- visual style
- layout
- spacing
- typography
- design system
- component appearance
- page structure
- interaction feel
- user flow presentation
- responsive layout expectations

# When this skill should NOT trigger

Do not trigger this skill when the task is mainly about:
- requirement clarification
- project scoping
- technical architecture
- backend logic
- database design
- frontend implementation details unrelated to design
- testing
- deployment readiness

# Inputs expected

This skill expects some or all of:
- product goal
- target users
- platform type
- brand direction, if any
- examples the user likes or dislikes
- accessibility expectations
- device priority (desktop-first, mobile-first, responsive)

If key visual inputs are missing, ask the user concise and high-value questions.

# Required questioning behavior

Ask only the minimum questions needed to lock design direction.

Prioritize questions in this order:
1. overall style direction
2. target audience and tone
3. color preferences or restrictions
4. layout density preference
5. reference products or screenshots
6. accessibility or usability constraints

If the user is unsure, propose 2–3 design directions with tradeoffs.

# Outputs

Create or update these files when useful:
- `docs/ui-style-guide.md`
- `docs/page-structure.md`
- `docs/component-spec.md`

# Output requirements

## `docs/ui-style-guide.md`
Should include:
- product tone
- visual keywords
- color direction
- typography direction
- spacing and radius rules
- shadows, borders, and surfaces
- icon/image usage guidance
- responsiveness notes
- accessibility notes

## `docs/page-structure.md`
Should include:
- primary pages
- page goals
- layout blocks per page
- navigation structure
- important interaction patterns
- empty/loading/error state expectations

## `docs/component-spec.md`
Should include:
- reusable component list
- variants and states
- style rules
- behavior notes
- usage constraints

# Constraints and non-goals

Non-goals:
- building the final implemented frontend
- changing product requirements
- changing technical architecture
- inventing backend contracts

Constraints:
- design decisions should support implementation simplicity
- prefer reusable patterns over one-off decorative ideas
- avoid over-design
- document enough for implementers to follow consistently

# Workflow

1. Read existing planning documents.
2. Identify missing UI/UX decisions.
3. Ask concise clarifying questions only if needed.
4. Propose a coherent visual direction.
5. Define page structure and key layout decisions.
6. Define component system and major states.
7. Write or update design documentation.
8. Summarize decisions clearly for the next skill.

# Handoff expectations

Hand off to:
- `repo-architect` for technical interpretation of the UI direction
- `frontend-implementer` for implementation

Your handoff must clearly state:
- chosen design direction
- page structure
- reusable components
- key interaction rules
- responsive expectations
- unresolved design questions, if any