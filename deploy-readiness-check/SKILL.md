---
name: deploy-readiness-check
description: Evaluate whether a web project is ready to deploy by reviewing environment setup, build readiness, release blockers, and operational assumptions. Use this skill for deployment readiness assessment, not for writing core feature code or leading the main testing workflow.
---

# Purpose

This skill assesses whether the current project state is ready for deployment.

Use this skill to:
- review release readiness
- review environment variable completeness
- review build assumptions
- review CI/CD assumptions
- review hosting or container readiness assumptions
- identify release blockers
- produce a deployment checklist

Do not use this skill to:
- implement core product features
- redefine requirements
- act as the primary tester
- replace infrastructure engineering beyond readiness assessment

# When this skill should trigger

Trigger this skill when the task involves:
- deployment readiness
- pre-release checks
- environment setup completeness
- release blockers
- build packaging readiness
- Docker or container assumptions
- CI/CD readiness
- launch checklist generation

# When this skill should NOT trigger

Do not trigger this skill when the task is mainly about:
- building product features
- requirement clarification
- UI design
- writing the full testing strategy
- architecture planning unless needed for release assessment

# Inputs expected

This skill expects:
- project scope
- implementation summary
- test summary
- build commands
- deployment target assumptions
- environment/config expectations
- Docker/hosting/CI files if they exist

# Outputs

Create or update:
- `docs/deploy-checklist.md`

Optional:
- `docs/release-blockers.md`

# Output requirements

## `docs/deploy-checklist.md`
Should include:
- required environment variables
- build commands
- start/run commands
- database or migration requirements
- asset or static build requirements
- external service dependencies
- CI/CD assumptions
- logging/monitoring assumptions
- rollback considerations if relevant
- open blockers

# Constraints and non-goals

Non-goals:
- becoming a full DevOps replacement
- silently creating a production architecture redesign
- masking missing validation

Constraints:
- distinguish confirmed readiness from assumptions
- explicitly list unknowns
- prefer practical release blockers over abstract concerns
- tie every blocker to an action or owner where possible

# Workflow

1. Read implementation and test summaries.
2. Inspect deployment-related files if present.
3. Identify build, runtime, and environment dependencies.
4. Identify missing secrets, services, or setup requirements.
5. Write the readiness checklist.
6. Separate confirmed-ready items from unknown or blocked items.
7. Summarize release confidence and blockers.

# Handoff expectations

Hand off to:
- `workflow-orchestrator`
- optionally implementation skills if blockers require code changes

Your handoff must clearly state:
- ready items
- blocked items
- unknown items
- minimum next steps before deployment