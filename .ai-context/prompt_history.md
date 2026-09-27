# Prompt History

This is an append-only audit log of user prompts and material execution events.

## 2026-09-27 — INT SDD Line 8 Architecture and Repository Standards

- **Artefact/task ID:** SDD Bootstrap — Line 8 Architecture and Repository Governance
- **Objective:** Audit repository for Lines 1–7 completeness; establish the INT Line 8 Architecture and Repository Standards governance layer. No application functionality implemented. No architecture invented.
- **Files created:**
  - `.agent/rules/int-sdd-architecture.md` — authoritative 21-section rule covering: mandatory plan→constitution check, integration-point requirement, data-model requirement, deferred-item requirement, ADR significance test (INT one-day-rework rule), ADR-as-WHY/plan-as-HOW, project-global ADR IDs, architecture.md as living document, architecture change propagation, spec-bypass prohibition, trivial-change exception, one-feature-branch-per-spec, branch naming, branch scope, squash-merge, AI-attribution prohibition, human accountability, PR traceability, .agent/.ai-context placement, and repository structure.
  - `.agent/workflows/architecture-check.md` — 8-part architecture check workflow: constitution compliance, integration points, data model, deferred items, architectural impact table, ADR requirement assessment, architecture.md currency, spec bypass. Produces ARCHITECTURE CHECK — PASS / BLOCKED report.
  - `.agent/workflows/adr-check.md` — 6-step ADR assessment: is-it-architectural, significance test, existing-ADR search, architecture.md coverage, downstream impact, assessment output. Includes ADR template.
  - `.agent/workflows/repository-check.md` — 8-part repository check: slug consistency, branch naming, branch scope, PR traceability, commit compliance, squash-merge, internal artefact control, human accountability. Produces REPOSITORY CHECK — PASS / BLOCKED report.
  - `.ai-context/decisions/_template.adr.md` — canonical ADR template (Status, Date, Context, Decision, Alternatives, Consequences, Downstream Impact, Related ADRs, References, Gate Record).
- **Files updated:**
  - `.agent/workflows/generate-plan.md` — strengthened: mandatory derivation chain with ARCHITECTURE CHECK gate, explicit requirements for all five mandatory plan sections, cross-references to architecture-check.md and adr-check.md.
  - `.ai-context/plans/_template.plan.md` — rebuilt with all required INT Line 8 sections: Integration Points, Data Model Impact, Constitution Compliance checklist, Significant Architectural Decisions, Deferred Implementation Items.
  - `.ai-context/architecture.md` — added Governance header (living document rules, update triggers, how-to-update, governing rule reference).
  - `.ai-context/constitution.md` — added Lines 6–9 governance cross-references in Non-Negotiable Gates section.
  - `README.md` — added Architecture and Repository Standards section.
  - `.ai-context/status.md` — recorded Line 8 activity.
- **Validation:** Audited Lines 1–7; no conflicts; lifecycle.md Architecture Check stage is now backed by a concrete workflow; no fake architecture, fake ADRs, or application functionality created.
- **Unresolved issues:** Business requirements, project stack, architecture, reviewer assignments, security posture remain TBD.
- **Decisions/deviations:** No application functionality implemented. No architecture invented. No fake ADRs created.

## 2026-09-25 — Bootstrap Initialization


- **Request:** Establish a clean INT Specification-Driven Delivery workspace without implementing application functionality or inventing requirements or architecture.
- **Outcome:** Repository inspected; SDD control-plane and context artefacts initialized. No feature specification, plan, task, test, application code, or dependencies were created.

## 2026-09-25 — AI Operating Model Setup

- **Artefact/task ID:** SDD Bootstrap — AI operating model
- **Objective:** Establish repository-based persistent context, scoped agent execution, stable identity prompting, ambiguity handling, model independence, session boundaries, and reusable SDD workflows.
- **Files changed:** `.agent/rules/int-standards.TBD.md`, `.agent/rules/.agentignore`, `.agent/rules/auto-log.md`, `.agent/workflows/generate-plan.md`, `.agent/workflows/generate-tests.md`, `.agent/workflows/code-review.md`, `.ai-context/constitution.md`, `.ai-context/project_context.md`, `.ai-context/status.md`, `.ai-context/prompt_history.md`.
- **Validation:** Reviewed the control-plane and context structure; confirmed workflows, audit mechanism, scoped-context rules, and no-invention rules are present.
- **Unresolved issues:** Project purpose, business requirements, technology stack, architecture, reviewers, security, and non-functional constraints remain TBD.
- **Decisions/deviations:** Stack-specific standards remain intentionally unresolved; no framework or architecture was selected.

## 2026-09-25 — Core SDD Artefact Model Setup

- **Artefact/task ID:** SDD Bootstrap — artefact model
- **Objective:** Formalize Constitution → Spec → Plan → Tasks derivation, artefact ownership and boundaries, stable identifiers, Gate dependencies, and reusable future-feature templates.
- **Files changed:** `.ai-context/constitution.md`, `.ai-context/specs/_template.spec.md`, `.ai-context/plans/_template.plan.md`, `.ai-context/tasks/_template.tasks.md`, `.agent/workflows/generate-plan.md`, `.agent/workflows/generate-tests.md`, `.agent/workflows/code-review.md`, `.ai-context/status.md`, `.ai-context/prompt_history.md`.
- **Validation:** Audited the artefact chain, upstream/downstream rules, templates, workflow dependencies, identifier convention, and absence of feature content.
- **Unresolved issues:** Business requirements, project purpose, technology stack, architecture, reviewer assignments, security posture, and non-functional constraints remain TBD.
- **Decisions/deviations:** Templates use placeholders only; no feature requirement, architecture decision, or implementation was created.

## 2026-09-25 — Non-Negotiable SDD Enforcement Setup

- **Artefact/task ID:** SDD Bootstrap — enforcement layer
- **Objective:** Establish hard gates for approved-spec-only implementation, anti-vibe-coding, confirmed test RED, sensitive-data exclusion, scoped context, and human accountability.
- **Files changed:** `.agent/rules/int-sdd-non-negotiables.md`, `.agent/rules/auto-log.md`, `.agent/workflows/pre-implementation-compliance.md`, `.agent/workflows/generate-tests.md`, `.agent/workflows/code-review.md`, `.ai-context/constitution.md`, `.ai-context/project_context.md`, `.ai-context/status.md`, `README.md`, `.ai-context/prompt_history.md`.
- **Validation:** Audited the six enforcement rules, pre-implementation and merge checks, workflow references, context boundary, sensitive-data controls, and absence of application artefacts.
- **Unresolved issues:** No feature has a validated BRD, approved spec, named Gate 1 reviewer, plan, tasks, reviewed tests, or RED evidence; implementation is intentionally blocked.
- **Decisions/deviations:** No sensitive data was persisted; no feature work or application functionality was created.

## 2026-09-27 — INT SDD Line 7 Gate 1 Peer Review

- **Artefact/task ID:** SDD Bootstrap — Line 7 Gate 1 Governance
- **Objective:** Audit repository for Lines 1–6 completeness; establish the INT Line 7 Gate 1 Peer Review governance mechanism as a mandatory, enforceable, reviewer-facing standard. No application functionality implemented.
- **Files created:**
  - `.agent/rules/int-sdd-gate-1.md` — authoritative Gate 1 rule: 14 sections covering what Gate 1 is/is not, reviewer requirements (named, non-author, API-surface SME consideration, security/architecture sign-off), seven review dimensions (ambiguity, testability, scope, API contract, constitution compliance, overlap, dependencies), plan review, architecture impact assessment, outcomes (Approved/Changes Requested), structured finding format with dimension codes, blocking vs non-blocking criteria, turnaround expectation, revision traceability, automated vs human approval distinction, and status integration.
  - `.agent/workflows/gate-1-review.md` — reviewer-facing Gate 1 workflow: 11-part structured checklist (identity/readiness, BRD/discovery, intent, acceptance criteria testability, failure paths, scope, API contract, constitution, overlap, dependencies, architecture impact, plan, safety), reviewer challenge questions for each dimension, structured finding format, standardized Gate 1 report template with self-approval prohibition confirmation, and critical reminders.
- **Files updated:**
  - `.ai-context/constitution.md` — added Gate 1 peer review (Line 7) subsection and Gate 1 governance references in Non-Negotiable Gates section.
  - `README.md` — added Gate 1 Peer Review section with rule and workflow references.
  - `.ai-context/specs/_template.spec.md` — strengthened Gate Approvals section with three-event distinction (automated readiness / reviewer analysis / human approval), gate-1-review.md reference, and updated table format.
  - `.ai-context/status.md` — recorded Line 7 activity.
- **Validation:** Audited all Lines 1–6 artefacts; confirmed no conflicts introduced; confirmed distinction between automated spec-review.md (author-facing pre-submission) and gate-1-review.md (reviewer-facing human gate); confirmed no feature spec, BRD entry, plan, task, test, or application code was created; confirmed all Line 7 requirements are represented.
- **Unresolved issues:** Business requirements, project stack, architecture, Gate 1/Gate 2 reviewer assignments, security posture, and non-functional constraints remain TBD.
- **Decisions/deviations:** No application functionality implemented. No real feature slug, BRD entry, or requirements invented.

## 2026-09-27 — INT SDD Line 6 Specification Standard

- **Artefact/task ID:** SDD Bootstrap — Line 6 Specification Standard
- **Objective:** Audit repository for Lines 1–5 completeness; establish the INT Line 6 Specification Standard as a mandatory, enforceable repository governance layer without implementing any feature or application functionality.
- **Files created:**
  - `.agent/workflows/spec-prewriting-gate.md` — mandatory nine-question pre-writing readiness gate.
- **Files updated:**
  - `.ai-context/specs/_template.spec.md` — rebuilt to the canonical INT Line 6 structure (`# Spec: <Feature Name>`, correct section order, conditional API contract, failure-path guidance, anti-pattern warnings, explicit Boundary section).
  - `.agent/workflows/spec-review.md` — expanded with full quality gate checklist (BLOCKING markers), anti-pattern block table, spec change propagation check, structured CHANGES REQUIRED output format, and explicit self-approval prohibition.
  - `.agent/rules/int-sdd-specification-standard.md` — expanded from outline to 18 numbered sections covering all INT Line 6 requirements: feature boundary, slug standard, pre-writing gate, BRD→Spec relationship, spec purpose, content order, intent standard, AC standard, failure paths, API contract rule, explicit out-of-scope rule, test case standard, NFC sourcing, status/versioning, change propagation, Gate 1 readiness, anti-patterns, context reference, implementation leak prevention.
  - `.ai-context/constitution.md` — added Specification Standard (Line 6) subsection and three-stage governance layer reference in Non-Negotiable Gates.
  - `README.md` — added Specification Standard section with three-stage governance references.
  - `.ai-context/status.md` — recorded Line 6 activity.
- **Validation:** Audited all Lines 1–5 artefacts; confirmed no conflicts introduced; confirmed no feature spec, real BRD entry, plan, task, test, or application code was created; confirmed all Line 6 requirements from the INT SDD Blueprint are now represented in repository artefacts.
- **Unresolved issues:** Business requirements, project stack, architecture, reviewer assignments, security posture, and non-functional constraints remain TBD.
- **Decisions/deviations:** No application functionality implemented. No real feature slug created. No requirements invented.

## 2026-09-25 — Complete SDD Lifecycle Setup

- **Artefact/task ID:** SDD Bootstrap — lifecycle governance
- **Objective:** Establish the controlled INT delivery lifecycle, state transitions, stage evidence, gates, lifecycle validation, release/support feedback, and compressed hotfix handling.
- **Files changed:** `.ai-context/lifecycle.md`, `.agent/workflows/sdd-lifecycle-check.md`, `.ai-context/constitution.md`, `.ai-context/project_context.md`, `.ai-context/status.md`, `.ai-context/tasks/_template.tasks.md`, `.agent/rules/int-sdd-non-negotiables.md`, `README.md`, `.ai-context/prompt_history.md`.
- **Validation:** Audited discovery, BRD, constitution, spec, Gate 1, plan, architecture check, tasks, RED, GREEN, Gate 2, merge, release, support, feedback, hotfixes, state transitions, and status-board coverage.
- **Unresolved issues:** No validated BRD, feature, reviewer roster, release convention, production-support process, technology stack, or architecture exists yet; lifecycle execution remains blocked at bootstrap.
- **Decisions/deviations:** The lifecycle reference uses governance placeholders only; no real feature artefact, hotfix, production record, or implementation was created.
