---
name: pr-feedback-resolver
description: Convert pull request review comments or issue feedback into an actionable resolution plan, track comment-by-comment status, implement selected fixes when requested, and draft concise replies. Use when the user wants to address PR feedback from pasted comments, GitHub data, or review threads, not for general code review or CI failure triage.
---

# PR Feedback Resolver

## What It Does

- Turns review comments into a clear action list.
- Groups duplicate or related feedback.
- Distinguishes required changes, clarification questions, optional suggestions, and non-actionable comments.
- Implements selected fixes when the user asks for code changes.
- Drafts concise replies that explain what changed or why no change was made.

## When It Should Trigger

- The user asks to address PR comments, review feedback, requested changes, or issue comments tied to a branch.
- The user pastes review comments and wants them resolved.
- A GitHub or GitLab PR has actionable comments that need triage and follow-up.
- The work is comment-driven rather than a fresh implementation request.

## When It Should Not Trigger

- The user asks for a general code review; use `code-reviewer`.
- CI or PR checks are failing; use `ci-failure-triager`.
- The requested change is already clearly scoped and no feedback triage is needed; use the relevant implementer.
- The feedback is mainly product scope, UX, architecture, security, or test strategy; route to the specialist skill.

## Expected Inputs

- Pasted comments, review thread text, issue comments, or PR URL.
- Current branch or diff if available.
- User preference on which comments to address when there are many.
- Repository instructions and relevant files.

## Expected Outputs

- Numbered feedback inventory with status.
- Selected action plan.
- Code changes when requested and feasible.
- Reply drafts for resolved comments.
- Verification summary and remaining unresolved comments.

## Preflight

- If using a hosted PR provider, check whether the needed CLI or connector is available and authenticated.
- If no provider access exists, work from pasted comments or locally available files.
- Do not require GitHub API access when the user has supplied enough comment text to proceed.
- Do not mark remote threads resolved unless the user explicitly asks and the required access is available.

## Workflow

### 1. Collect Feedback

- Gather comments from pasted text, local notes, or provider tooling.
- Preserve enough context to understand file, line, reviewer intent, and requested behavior.
- Number comments so the user can approve or skip specific items.

### 2. Classify Comments

Classify each item as:

- required fix
- bug report
- missing test
- clarification needed
- optional improvement
- duplicate
- out of scope
- already addressed

### 3. Build A Resolution Plan

- Group related comments into one fix when appropriate.
- Identify the likely owner skill for each group.
- Ask for selection only when comments are numerous, ambiguous, or potentially out of scope.
- Do not silently implement optional or controversial changes.

### 4. Apply Selected Fixes

- Read the relevant files and tests before editing.
- Keep fixes scoped to the selected comments.
- Avoid unrelated cleanup.
- Preserve user changes and existing branch work.

### 5. Verify And Draft Replies

- Run focused checks tied to the addressed feedback.
- For each comment, state status: fixed, answered, skipped, blocked, or needs user decision.
- Draft short replies that mention the concrete change or reason.

## Output Shape

```md
# PR Feedback Resolution

## Feedback Inventory
- [1] Status - Comment summary and source

## Changes Made
- Files and behaviors changed

## Verification
- Commands or checks run

## Replies
- [1] Draft reply

## Remaining
- Unresolved comments or blockers
```

## Handoff Expectations

Route to:

- `backend-implementer` or `frontend-implementer` for substantive code fixes
- `test-engineer` for requested coverage
- `code-reviewer` for a final review pass
- `ci-failure-triager` if checks fail after changes
- `web-security-reviewer` if feedback touches auth, permissions, or sensitive data

## Non-Goals

- General code review without supplied feedback
- CI log investigation
- Broad refactoring beyond review comments
- Automatically resolving remote comments without explicit user approval
- Re-litigating product scope unless the comment requires it
