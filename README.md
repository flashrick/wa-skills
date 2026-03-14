# Repo-Local Agent Skills

These skills are narrow, metadata-first skeletons for a web project. Their `name` and `description` fields are intentionally specific so skill discovery can route work to the right specialist with minimal overlap.

## Skills

| Skill | Focus |
| --- | --- |
| `project-manager` | Scope, milestones, acceptance criteria, delivery slices |
| `repo-architect` | Repository layout, module boundaries, interface seams |
| `uiux-designer` | User flows, interaction states, layout intent, visual direction |
| `backend-implementer` | APIs, business logic, persistence, backend integrations |
| `frontend-implementer` | Screens, components, client state, browser interactions |
| `test-engineer` | Automated tests and targeted regression coverage |
| `deploy-readiness-check` | Pre-deploy operational readiness and release risk review |
| `workflow-orchestrator` | Cross-skill sequencing, handoffs, and parallel work boundaries |

## Recommended Stage Order

1. `workflow-orchestrator` when the request spans multiple specialties and needs coordination.
2. `project-manager` to lock scope, milestones, and acceptance criteria.
3. `repo-architect` to define repo structure and interface boundaries.
4. `uiux-designer` to specify user-facing flows and states.
5. `backend-implementer` to build server-side behavior.
6. `frontend-implementer` to build client-side behavior.
7. `test-engineer` to add focused automated validation.
8. `deploy-readiness-check` before release or handoff to deployment.

## Notes

- Use the most specific skill that cleanly matches the task.
- Use `workflow-orchestrator` only when multiple specialist skills need coordination.
- No `scripts/` are included yet by design.
