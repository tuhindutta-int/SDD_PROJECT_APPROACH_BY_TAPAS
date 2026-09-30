# Prompt History

This is an append-only audit log of user prompts and material execution events.

## 2026-09-30 — INT SDD Line 10 Development Discipline

- **Artefact/task ID:** SDD Bootstrap — Line 10 Development Discipline
- **Objective:** Perform a full repository audit and establish INT SDD Line 10 as an authoritative, enforceable development discipline governance layer. Core rule: one task, one prompt. Generate → review → commit. No application functionality implemented. No architecture invented.
- **Phase 1 — Inspect:** Full audit of `.agent/rules/`, `.agent/workflows/`, all templates, `constitution.md`, `lifecycle.md`, `status.md`, `prompt_history.md`, and all artefact chain conventions. Lines 1–9 confirmed present.
- **Phase 2 — Lines 1–9 verification:** Confirmed present. Line 10-relevant provisions existed but were scattered and insufficient:
  - `pre-implementation-compliance.md`: had task ID, scoped context, RED evidence, one-task reference — but no generate→review→commit structure
  - `int-sdd-non-negotiables.md`: had "one bounded task at a time" and "do not re-prompt" — but no failure source classification or upstream correction path
  - `code-review.md`: 27-line thin workflow; no task-scoped structure, no "significantly wrong" classification, no escalation path
  - `lifecycle.md`: "one task, scoped context" in the In Development row — but one line
  - Tasks template: basic states and one-task execution note — but no readiness fields, prompt template, or test-first prerequisites table
- **Phase 3 — Gaps identified:**
  - No authoritative `int-sdd-development.md` rule file (Line 10 had no canonical home)
  - No `task-by-task-development.md` operationalized workflow
  - No `development-discipline-check.md` validation workflow
  - `code-review.md` was thin and not task-scoped
  - No definition of "significantly wrong" generation and stop rule
  - No upstream escalation path (prompt → task → plan → spec → ADR)
  - No failure source classification (A–G taxonomy)
  - No bounded corrective prompting principle
  - No changeset boundary definition with explicit scope test
  - No anti-pattern block (mega-prompt, aggregate diff, vibe coding)
  - No model-to-task matching rule
  - Tasks template lacked: implementation readiness fields, test-first prerequisites table, prompt template, explicit non-goals, context scope column
- **Files created:**
  - `.agent/rules/int-sdd-development.md` — 16-section authoritative development discipline rule: core contract (one-task/one-prompt/one-diff/one-commit), compliant/non-compliant prompt patterns, task readiness checklist (12 items, 3 categories of blocking conditions), small verifiable increments with prohibited compression patterns, changeset discipline (explicit scope test), review-before-commit with 12-item checklist, significantly-wrong generation stop rule with 7-source failure taxonomy (A–G) and upstream correction path, context discipline, prompt-by-identity integration, test-first compatibility, commit governance (AI attribution prohibition), model-to-task matching, task status integration, human accountability, anti-pattern table (11 entries), cross-line integration map.
  - `.agent/workflows/task-by-task-development.md` — 12-step operationalized task execution loop: 3-part preconditions (full SDD chain, test-first prerequisites, status board); main loop: select→verify readiness→load context→update status→prompt by identity→generate→inspect diff→review→classify outcome (4 classifications with escalation)→commit→update status→next task; failed change handling; anti-pattern reference table; all 13 Blueprint scenarios validated against specific steps.
  - `.agent/workflows/development-discipline-check.md` — 6-part validation workflow: Part 1 task readiness (4 subsections, 20+ checks), Part 2 changeset scope (explicit changeset test, file-by-file triage), Part 3 review completeness (7 subsections), Part 4 commit compliance (message format, AI attribution detection, scope, status), Part 5 status accuracy, Part 6 significantly-wrong generation classification (7-source diagnostic + escalation table, excessive re-prompting detection). Structured finding format with 11 area codes.
- **Files updated:**
  - `.agent/workflows/code-review.md` — Replaced 27-line thin workflow with comprehensive task-scoped review workflow: task-scoped review question, 12-item review procedure, AC/API/test/security/architecture/deferred-scope/changeset/unexpected-files/behaviour/unrelated checks, outcome classification (4 states), structured review record, completion signal. Explicit Gate 2 distinction.
  - `.ai-context/tasks/_template.tasks.md` — Added: explicit state transition definitions (generated≠reviewed≠merged), Execution Rules section, Context Required column, Test-First Prerequisites table with RED confirmation tracking, Implementation Prompt Template using stable IDs, reinforced Boundary section.
  - `.agent/workflows/pre-implementation-compliance.md` — Added Line 10 governance cross-references (development rule, execution workflow, discipline check).
  - `.agent/rules/int-sdd-non-negotiables.md` — Strengthened Scope/Ambiguity/Consistency Gates section with one-task-one-prompt, generate→review→commit, and failure source classification cross-references.
  - `.ai-context/constitution.md` — Added Line 10 governance items 13–16 (development rule, execution workflow, discipline check, code review).
  - `README.md` — Added Development Discipline (Line 10) section with cycle diagram and significantly-wrong stop rule.
  - `.ai-context/status.md` — Line 10 activity recorded.
- **Lines 1–9 cross-line compatibility:** All verified. No conflicts introduced. Line 10 is additive:
  - Line 4 test-first preserved: RED must precede generate step
  - Line 5 lifecycle preserved: task states used verbatim (Not Started → In Progress → In Review → Merged)
  - Line 8 branch/commit standards preserved: commit traceability pattern uses task ID; AI attribution prohibition reinforced
  - Line 9 naming/ID standards integrated: prompt-by-identity (`<slug>.TNN`, `<slug>.ACN`) used throughout
  - Gate 2 preserved: task-level review (pre-commit) is explicitly distinguished from Gate 2 (pre-merge)
- **Validation:** All 20 quality-bar questions from task specification validated against repository evidence. All 13 Blueprint failure scenarios (A–M) validated against specific workflow steps.
- **Unresolved issues:** Business requirements, project stack, architecture, reviewer assignments remain TBD. No tasks exist to execute against — correct for bootstrap state.
- **Decisions/deviations:** No application functionality implemented. No example feature created. No fake tasks for demonstration. Repository remains a reusable SDD governance foundation.

## 2026-09-29 — INT SDD Line 9 Naming and Identifier Governance

- **Artefact/task ID:** SDD Bootstrap — Line 9 Naming and Identifier Governance
- **Objective:** Perform a full repository audit and establish INT SDD Line 9 as an authoritative, enforceable naming-and-identifier governance layer. No application functionality implemented. No architecture invented.
- **Phase 1 — Inspect:** Full audit of all `.agent/rules/`, `.agent/workflows/`, all templates, `constitution.md`, `status.md`, `prompt_history.md`, and all artefact chain conventions. Confirmed Lines 1–8 are present and internally consistent.
- **Phase 2 — Lines 1–8 reconstruction:** Verified against actual repository files. All Lines 1–8 confirmed present. Slug-related provisions existed in `int-sdd-specification-standard.md` §2, `int-sdd-architecture.md` §12–13, `constitution.md`, and `repository-check.md` §1 — but with scattered, partial coverage and no single authoritative source.
- **Phase 3 — Gaps identified:**
  - No authoritative naming/identifier rule (Line 9-specific)
  - No slug uniqueness/collision rules beyond casual "unique" mentions
  - No slug immutability or controlled-rename process
  - No slug archival/retirement formal rules
  - No ID immutability rules (AC/API/UT/T drift prevention)
  - No ID validity and cross-reference validation rules
  - No prompt-by-identity formal principle
  - No dedicated naming/identifier validation workflow
  - No test-case template existed anywhere in the repository
  - No slug registry (central record for collision enforcement)
  - Identity namespaces (feature-slug, BRD-NNN, ADR-NNNN) not formally distinguished
- **Files created:**
  - `.agent/rules/int-sdd-naming.md` — 14-section authoritative naming and identifier rule: slug as canonical root identity, format requirements, assignment timing, complete propagation chain, uniqueness/collision rules with slug registry, immutability, controlled rename process, retirement/archival, stable sub-identifier hierarchy (AC/API/UT/T), ID immutability (content vs identity change), three identity namespaces, cross-reference validity rules, prompt-by-identity governance, automated-checks-vs-human-approval distinction, Line 1–8 integration table.
  - `.agent/workflows/naming-identifier-check.md` — 13-part validation workflow covering all 22 required checks: slug existence/timing, format, uniqueness/collision (including registry search), immutability, artefact filename consistency, branch naming, PR traceability, status board, AC/API/UT/T sub-ID validity, cross-reference validity, ADR identity, retired-slug reuse, prompt-by-identity readiness. Structured finding format with 15 area codes. Failure scenario table mapping all 10 Blueprint scenarios to specific check parts.
  - `.ai-context/slug-registry.md` — Single authoritative slug record with Active and Retired/Archived/Renamed tables, status definitions, and registration instructions. The minimum required mechanism for reliable collision enforcement.
  - `.ai-context/test_cases/_template.test_cases.md` — Missing test-case template with Feature Slug header, TC-ID format (`<slug>.TC01`), AC/UT mapping columns, RED/GREEN tracking, failure path coverage table, and identity rules section.
- **Files updated:**
  - `.agent/workflows/spec-prewriting-gate.md` — Added Q0 slug assignment step as the mandatory first action before any spec content is drafted; includes slug registry confirmation and naming-check workflow reference.
  - `.agent/workflows/spec-review.md` — Strengthened Scope and Identity section with slug registry requirement, filename slug match, Spec ID header match, and naming-identifier-check workflow integration.
  - `.ai-context/constitution.md` — Feature traceability section expanded with slug-as-identity framing and prompt-by-identity principle; Line 9 governance cross-references (rules 10–12) added.
  - `README.md` — Feature Traceability Convention section replaced with comprehensive Naming and Identifier Governance section including traceability chain diagram and stable ID hierarchy table.
  - `.ai-context/status.md` — Line 9 activity recorded.
- **Validation:** Audited all Lines 1–9 for contradictions. All failure scenarios from Blueprint §27 validated against the workflow. No conflicts introduced. No fake architecture, fake ADRs, fake slugs, or application code created.
- **Unresolved issues:** Business requirements, project stack, architecture, reviewer assignments remain TBD. First real slug will be assigned when a validated BRD entry exists.
- **Decisions/deviations:** No application functionality implemented. No example feature created. Slug registry starts empty — correct for a bootstrap governance workspace.

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
