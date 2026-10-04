# Project Status Board

## Current State

| Field | Value |
| --- | --- |
| Current phase | SDD Bootstrap |
| Status | Initial Setup |
| Current owner | [UNDEFINED] |
| Feature spec approval | No feature spec approved |
| Implementation authorization | No implementation tasks authorized |

## Active Specs

| Spec ID | Title | Status | Owner | Last Updated | Notes / Blockers |
| --- | --- | --- | --- | --- | --- |

No active specifications.

## Blockers

- Validated business requirements have not been supplied.
- Project, technology, architecture, governance, and non-functional decisions remain unresolved.
- Normal implementation is blocked until a feature completes the approved SDD chain and pre-implementation compliance check.

## Open Decisions

- Project name, business objective, stakeholders, and users/personas.
- Project type, stack, architecture style, data/integration, security, and deployment choices.
- Gate 1 and Gate 2 reviewer assignments.

## Recent Activity

- 2026-10-04 — INT SDD Line 11 QA & Security Governance established: int-sdd-qa.md rule (13 sections: QA source-of-truth chain, AC→test derivation blocking rule, TC/UT namespace distinction, test-case structure, AI-generated test governance, QA judgement boundary, UAT traceability with specification-gap protocol, test-coverage gap detection, test-first preservation, integration/regression governance, QA failure escalation), int-sdd-security.md rule (13 sections: authorship independence, secrets/PII governance, dependency vetting, SAST/DAST/scanning governance with deferred-not-waived distinction, injection governance including prompt injection, authentication/authorization boundary distinction, least privilege, data protection, security exceptions, Gate 2 security checklist with 16 controls, external/untrusted content governance, security failure escalation), qa-validation-check.md workflow (7-part: test derivation, AC coverage, test-first RED evidence, AI-test review, UAT traceability, integration/regression, Gate 2 QA evidence), security-check.md workflow (8-part: secrets/PII, dependency vetting, scanning controls, injection, auth/authz, least privilege, data protection, Gate 2 security evidence with 16-control checklist), generate-tests.md replaced (8-step with derivation-source verification and STOP rule), _template.test_cases.md updated (Security notes, QA notes, broader QA scenarios, UAT traceability, AI-test review record), _integration.md created (cross-feature/regression), code-review.md §5 strengthened (full security control checklist), constitution.md (Testing Discipline and Security Posture strengthened from TBD placeholders; Line 11 governance items 17–22 added), README.md (Line 11 QA & Security section). No feature work created.

- 2026-09-30 — INT SDD Line 10 Development Discipline established: int-sdd-development.md rule (16 sections), task-by-task-development.md workflow (12-step loop), development-discipline-check.md validation workflow (6 parts, 18 checks), code-review.md strengthened (task-scoped, from 27 lines to comprehensive), tasks template updated (state definitions, test-first prerequisites table, prompt template), pre-implementation-compliance.md and int-sdd-non-negotiables.md updated with Line 10 cross-references, constitution.md (Line 10 governance items 13–16 added), README.md (Line 10 section). No feature work created.

- 2026-09-29 — INT SDD Line 9 Naming and Identifier Governance established: int-sdd-naming.md rule (14 sections), naming-identifier-check.md workflow (13-part, 22 validations), slug-registry.md (collision/retirement enforcement), test_cases/_template.test_cases.md (missing template created), spec-prewriting-gate.md (Q0 slug assignment step added), spec-review.md (slug registry and naming check requirements added), constitution.md and README.md updated with Line 9 cross-references. No feature work created.

- 2026-09-27 — INT SDD Line 8 Architecture and Repository Standards established: int-sdd-architecture.md rule (21 sections), architecture-check.md workflow (8-part), adr-check.md workflow (6-step ADR assessment + ADR template), repository-check.md workflow (8-part), ADR template, strengthened generate-plan.md and plan template, architecture.md governance header, constitution/README cross-references added. No feature work created.

- 2026-09-27 — INT SDD Line 7 Gate 1 Peer Review governance established: gate-1-review.md workflow (reviewer-facing, 11-part checklist, structured finding format, self-approval prohibition), int-sdd-gate-1.md rule (7 review dimensions, reviewer requirements, outcomes, architecture impact, turnaround, revision traceability), constitution/README/spec-template cross-references added. No feature work created.

- 2026-09-27 — INT SDD Line 6 Specification Standard established: spec pre-writing gate, spec quality gate checklist, anti-pattern block, spec change propagation rule, updated spec template (canonical INT structure), strengthened spec-review workflow, and constitution/README cross-references added. No feature work created.

- 2026-09-25 — Complete SDD lifecycle state model and lifecycle-check workflow established; no feature work created.

- 2026-09-25 — SDD workspace bootstrap initialized; no feature work created.
- 2026-09-25 — AI operating model established; no feature work created.
- 2026-09-25 — Core SDD artefact model and reusable templates established; no feature work created.
- 2026-09-25 — Non-negotiable SDD enforcement layer and pre-implementation compliance check established; no feature work created.
