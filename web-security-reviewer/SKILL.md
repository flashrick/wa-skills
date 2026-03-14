---
name: web-security-reviewer
description: Review a scoped web project, feature, or changed area for application security risk such as auth, permissions, input handling, trust boundaries, data exposure, browser security, and high-risk integrations. Use this skill for focused security review, not infrastructure architecture, full penetration testing, or primary feature implementation.
---

# Web Security Reviewer

## What It Does

- Reviews a scoped web project area or feature for application security risk.
- Identifies likely vulnerabilities, weak trust boundaries, risky defaults, and missing defensive controls.
- Produces a concise security review with prioritized findings, verification targets, and next actions.

## When It Should Trigger

- The user explicitly asks for a security review, vulnerability review, auth review, permission review, or attack-surface check.
- The work involves login, session handling, roles, permissions, tenant isolation, admin surfaces, or sensitive data access.
- The change introduces risky input or boundary crossings such as file upload, rich text, webhooks, external callbacks, redirects, HTML rendering, or third-party API trust.
- The feature affects cookies, JWTs, CORS, CSRF posture, headers, secret handling, or other browser-facing security controls.
- A release candidate needs a focused application-security review before deployment.

## When It Should Not Trigger

- The task is purely visual, copy-only, or otherwise has no meaningful security surface.
- The request is for network topology design, firewalling, cloud hardening, or full infrastructure security architecture.
- The user needs full feature implementation, broad architecture authorship, or primary test execution rather than scoped security review.
- The request is for a true penetration test or compliance certification beyond what repo-grounded review can support.

## Expected Inputs

- The scoped feature, changed files, or target repo area
- Existing `AGENTS.md`, if present
- Relevant auth, session, and permission behavior
- API contracts, request/response flows, or integration notes when available
- Existing tests, threat notes, or known risk concerns
- Deployment assumptions only when they affect application-level security behavior

## Expected Outputs

- A concise security review focused on the scoped area
- Prioritized security findings with severity and evidence
- Suggested mitigations or follow-up checks
- Optional `docs/security-review.md` or `docs/security-checklist.md`

## Review Priorities

Prioritize in this order:

1. auth, session, and permission failures
2. trust-boundary mistakes and missing authorization checks
3. data exposure and tenant isolation risk
4. unsafe input handling such as injection, XSS, SSRF, redirect, upload, or deserialization risk
5. browser security posture such as CSRF, cookies, CORS, and sensitive headers
6. dangerous defaults, secrets exposure, and weak logging or audit assumptions

Do not spend most of the review budget on low-risk style or generic code quality issues.

## Focus Areas

Inspect only the areas relevant to the scoped review. Common focus areas include:

- auth flow, session lifecycle, and logout invalidation
- permission enforcement on routes, API handlers, loaders, actions, and mutations
- tenant or account scoping for reads and writes
- validation and sanitization of external input
- file upload handling and content-type assumptions
- rich text or HTML rendering
- redirects, callback URLs, and webhook verification
- CORS, cookies, CSRF protections, and sensitive response headers
- secret leakage through code, config, logs, client bundles, or error messages

## Workflow

### 1. Frame The Security Surface

- Restate what is being reviewed and why it is security-sensitive.
- Identify the trust boundaries, privileged actions, sensitive data, and external inputs involved.
- Decide whether the review is feature-scoped, changed-area scoped, or release-scoped.

### 2. Read Only Relevant Context

- Inspect the changed files and directly related auth, data-access, validation, and integration code.
- Read `AGENTS.md` and relevant docs if they contain operational or boundary rules.
- Read nearby tests only when they reveal expected security behavior or missing coverage.
- Do not expand into a full-repo audit unless explicitly requested.

### 3. Trace Trust Boundaries

- Identify who can call what, with which data, under which assumptions.
- Check where user-controlled input crosses into privileged code, persistence, rendering, or outbound requests.
- Look for implicit trust in client state, IDs, headers, callback sources, or role claims.

### 4. Identify High-Risk Findings

- Prefer concrete, defensible findings tied to code paths or missing controls.
- Note severity in practical terms such as critical, high, medium, or low.
- Distinguish confirmed risk from uncertainty caused by missing context.

### 5. Recommend The Smallest Effective Follow-Up

- Suggest the narrowest mitigation, test, or validation step that materially reduces risk.
- Route implementation fixes to the appropriate implementer skill.
- Route validation follow-up to `test-engineer` when new tests or checks are needed.

## Questioning Strategy

- Ask questions only when they materially change the security judgment.
- Prefer reading actual code and config before asking how the system works.
- Keep questions about trust boundaries, auth assumptions, sensitive data, or privileged actions.
- Do not broaden into product discovery or generic architecture debate.

Use questions like these when needed:

- "Who should be allowed to perform this action?"
- "Does this route or handler rely on client-provided identity or role data?"
- "Can this input contain HTML, file content, external URLs, or callback targets?"
- "Is this feature single-tenant, role-scoped, or cross-tenant?"
- "What security controls are already assumed here: CSRF, signed URLs, webhook signatures, or rate limiting?"

## Output Shape

When producing a security review artifact, prefer this structure:

```md
# Security Review

## Scope
- What was reviewed
- Why it is security-sensitive

## Findings
- Severity
- Risk
- Evidence
- Recommended mitigation

## Must-Verify Behavior
- auth or permission behavior
- boundary conditions
- sensitive-data handling

## Follow-Up Checks
- tests to add
- manual or browser checks
- release-risk notes

## Open Uncertainty
- missing context that limits confidence
```

Keep the review concrete:

- tie every finding to a real behavior, code path, or missing control
- separate confirmed risk from inferred risk
- keep unreviewed areas visible

## Handoff Expectations

### To `backend-implementer` or `frontend-implementer`

Provide:

- the vulnerable or risky behavior
- the impacted trust boundary or sensitive action
- the recommended mitigation or control gap
- severity and likely abuse scenario when clear

Do not provide:

- vague "security issue" summaries without concrete risk
- broad redesign requests when a narrower fix exists

### To `test-engineer`

Provide:

- the security-sensitive behavior that should be validated
- the highest-value negative cases or authorization checks
- browser-level checks when CSRF, cookie, redirect, or click-surface behavior matters

Do not provide:

- a generic request to add more tests
- ownership of the security review itself

### To `deploy-readiness-check`

Provide only when release confidence depends on it:

- unresolved security blockers
- accepted risk or temporary mitigations
- configuration-sensitive concerns that affect deployment safety

## Non-Goals

- Full infrastructure, cloud, or network security architecture
- Full penetration testing or formal compliance audit
- Generic code review unrelated to security risk
- Primary feature implementation or broad test execution ownership
