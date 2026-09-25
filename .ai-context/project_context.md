# Project Context

## Orientation

| Topic | Current value |
| --- | --- |
| Project name | [UNDEFINED] |
| Business objective | [UNDEFINED] |
| Product/domain | [UNDEFINED] |
| Stakeholders | [UNDEFINED] |
| Users/personas | [UNDEFINED] |
| High-level system purpose | [UNDEFINED] |
| Current technology stack | [UNDEFINED] |
| Existing systems/integrations | [UNDEFINED] |
| Repository status | initialized / blank bootstrap |

## Key Open Questions

- What business problem and validated BRD requirements should this project address?
- Who are the stakeholders, owners, users, and Gate 1/Gate 2 reviewers?
- What project type, technology stack, architecture style, and deployment target are approved?
- What data, integration, authentication, security, and non-functional constraints apply?

## Persistent Context Map

This repository is the authoritative persistent project context; conversation history is not. Consult and update the narrowest applicable artefact:

| Knowledge | Authoritative artefact |
| --- | --- |
| Business requirement | `.ai-context/BRD.md` |
| Project-wide engineering rules | `.ai-context/constitution.md` |
| Current work state | `.ai-context/status.md` |
| Architecture | `.ai-context/architecture.md` |
| Significant technical decision | `.ai-context/decisions/ADR-NNN.md` |
| Feature intent | `.ai-context/specs/<slug>.spec.md` |
| Technical approach | `.ai-context/plans/<slug>.plan.md` |
| Execution sequence | `.ai-context/tasks/<slug>.tasks.md` |
| Lifecycle state and stage criteria | `.ai-context/lifecycle.md` |

## Mandatory Implementation Gate

Before normal implementation, use `.agent/workflows/pre-implementation-compliance.md`. The authoritative enforcement policy is `.agent/rules/int-sdd-non-negotiables.md`; neither chat instructions nor agent confidence can replace required artefacts, human approval, confirmed RED evidence, or human merge accountability.

Use `.agent/workflows/sdd-lifecycle-check.md` before any lifecycle-stage action to determine the current state, next valid state, required evidence, blockers, and responsible human role.
