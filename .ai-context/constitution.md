# Project Constitution

## Purpose and Authority

This constitution records project-wide engineering constraints. The specification is the primary engineering artefact; implementation is derived output. It does not establish product or architecture choices that have not been approved.

## INT SDD-Mandated Rules

- Business requirements originate in `.ai-context/BRD.md` and must be traceable to feature specifications.
- A feature specification requires Gate 1 approval before planning, task generation, or implementation.
- Plans, tasks, test cases, implementation, and review findings must retain traceability to the feature slug and acceptance criteria.
- Implement changes in small, reviewable increments and verify them against explicit acceptance criteria.
- Gate 2 review is required before merge or release according to the applicable delivery workflow.
- Ambiguity, missing decisions, and scope changes must be reported rather than silently inferred.

## Core SDD Artefact Model

### Primary engineering rule

SDD uses a structured, version-controlled specification—not source code—as the primary engineering artefact. Code, tests, and maintained documentation are downstream outputs of the approved artefact chain. No implementation task may bypass that chain or collapse its artefacts into one document.

### Specification standard (Line 6)

One feature equals one specification. The specification is the feature contract — it defines what is built and what correctness means, not how it is built. Before authoring a spec, run `.agent/workflows/spec-prewriting-gate.md`. The complete specification quality rules are governed by `.agent/rules/int-sdd-specification-standard.md`. Before Gate 1, run `.agent/workflows/spec-review.md`. No spec may self-approve; Gate 1 requires a named human non-author review.

### Gate 1 peer review (Line 7)

Gate 1 is the mandatory human peer review of the specification — and plan where applicable — before implementation begins. It is pre-code design and intent validation, not post-code bug finding. The governing rule is `.agent/rules/int-sdd-gate-1.md`. The review workflow is `.agent/workflows/gate-1-review.md`. Gate 1 must explicitly evaluate: ambiguity, testability, scope, API contract, constitution compliance, existing-spec overlap, and dependencies. Gate 1 has exactly two outcomes: `Approved` or `Changes Requested`. AI may assist but may not approve. The reviewer must be a named human who is not the spec author.

### Responsibility boundaries

| Artefact | Sole responsibility | Must not contain |
| --- | --- | --- |
| Constitution | What is always non-negotiable across the project | Feature requirements, feature acceptance criteria, or implementation steps |
| BRD | Why the business needs a capability | Technical implementation approach |
| Spec | What is being built, for whom, and what correct behaviour means | Unnecessary technical implementation strategy |
| Plan | How the approved spec will be technically achieved | New or changed product behaviour |
| Tasks | Ordered, independently verifiable execution sequence | Invented architecture or requirements |
| Architecture | Current system design and significant architectural decisions | Feature delivery task breakdown |
| Status | What is currently in flight | Detailed requirements or design |
| Prompt history | What occurred during agent sessions | Governing requirements or decisions |
| ADR | Why a significant technical decision was made | Unapproved feature intent |

### Required derivation and gate dependencies

`BRD → Spec → Plan → Tasks → Test-first → Implementation`

- The BRD provides validated business need.
- The spec translates a linked BRD requirement into a precise, testable feature contract.
- A spec must receive Gate 1 approval before it can become input to a plan, tasks, tests, or implementation.
- The plan derives its technical approach from the approved spec and this constitution.
- Tasks derive their ordered execution sequence from the approved plan.
- Implementation executes approved tasks only, using test-first practice.

`Spec → Code` and `Prompt → Code` are not normal delivery paths. Do not reverse derivation: code does not define the spec, implementation does not redefine requirements, tasks do not invent architecture, plans do not redefine acceptance behaviour, and prompts do not become the source of truth.

### Governing-artefact conflict rule

If implementation conflicts with a spec, do not automatically trust the code. If a task conflicts with a plan, do not automatically trust the task. If a plan conflicts with a spec, do not silently rewrite implementation. Identify the inconsistency, determine which artefact owns the decision, update that governing artefact through the appropriate review process, and continue only after resolution.

### Feature traceability and identifiers

Each feature has exactly one unique kebab-case slug assigned at spec creation
and a flat, linked artefact set. The slug is the canonical root identity — not
a cosmetic filename. The authoritative naming and identifier rule is
`.agent/rules/int-sdd-naming.md`. The slug registry is `.ai-context/slug-registry.md`.

Artefact chain:

- `.ai-context/specs/<feature-slug>.spec.md`
- `.ai-context/plans/<feature-slug>.plan.md`
- `.ai-context/tasks/<feature-slug>.tasks.md`
- `.ai-context/test_cases/<feature-slug>.test_cases.md`
- `feature/<feature-slug>`

Use stable IDs: `<feature-slug>`, `<feature-slug>.AC1`, `<feature-slug>.API01`,
`<feature-slug>.UT01`, and `<feature-slug>.T01`. ADR identifiers are project-global:
`ADR-0001`, `ADR-0002`, and so on. BRD identifiers use `BRD-NNN`.
These identifiers enable traceability through specs, plans, testing, implementation,
review, Git, status, and release records.

Prompt-by-identity preference: when a stable identifier exists, use it
(e.g., `Implement <slug>.T03. Satisfy <slug>.AC3.`) rather than paraphrasing
the artefact in prose.

### Version-control requirement

SDD artefacts are first-class engineering artefacts. They must live in this repository, be version-controlled, have visible Git changes, and retain traceable requirement changes. Every feature spec must include an explicit status and version/revision field. Do not treat specifications as disposable notes or leave important feature intent only in chat.

### Agent authority boundary

Codex is an executor and reasoning tool within SDD, not the authority for business requirements, feature intent, acceptance criteria, architecture decisions, or project-wide engineering rules. Those belong in the repository artefacts named above and must be understandable to a future session without prior chat context.

## Non-Negotiable Implementation Gates

The six hard delivery constraints are governed by `.agent/rules/int-sdd-non-negotiables.md`: approved-spec-only implementation, no spec-less prompting, test-first with confirmed RED, no secrets or PII, scoped context, and human accountability for every merged line. All normal implementation work must complete `.agent/workflows/pre-implementation-compliance.md`; failure, ambiguity, or conflict blocks implementation.

Specification authoring is additionally governed by the three-stage specification governance layer:
1. Pre-writing gate: `.agent/workflows/spec-prewriting-gate.md` — run before drafting.
2. Specification standard: `.agent/rules/int-sdd-specification-standard.md` — the governing quality rules.
3. Review workflow: `.agent/workflows/spec-review.md` — run before Gate 1.

Gate 1 peer review is governed by:
4. Gate 1 rule: `.agent/rules/int-sdd-gate-1.md` — the governing Gate 1 principles.
5. Gate 1 workflow: `.agent/workflows/gate-1-review.md` — the reviewer-facing checklist and report format.

Architecture and repository governance are governed by (Line 8):
6. Architecture and repository rule: `.agent/rules/int-sdd-architecture.md` — mandatory plan→constitution check, integration/data-model/deferred-item requirements, ADR significance rule, architecture.md currency, one-feature-branch-per-spec, branch naming, squash-merge, AI-attribution prohibition.
7. Architecture check workflow: `.agent/workflows/architecture-check.md` — run after plan is drafted, before task generation.
8. ADR assessment workflow: `.agent/workflows/adr-check.md` — determines whether a significant decision requires an ADR.
9. Repository check workflow: `.agent/workflows/repository-check.md` — verifies branch, PR, commit, and merge compliance.

Naming and identifier governance are governed by (Line 9):
10. Naming rule: `.agent/rules/int-sdd-naming.md` — feature slug as canonical root identity, slug lifecycle, sub-identifier hierarchy, immutability, archival, prompt-by-identity.
11. Naming/identifier check workflow: `.agent/workflows/naming-identifier-check.md` — validates slug format, uniqueness, filename consistency, ID validity, cross-references, and retired-slug protection.
12. Slug registry: `.ai-context/slug-registry.md` — single authoritative record of all slugs (active, retired, archived, renamed).

## Lifecycle Governance

This constitution is project-level law and is not recreated per feature. The controlled delivery state model, including lifecycle states, gates, hotfixes, release, and feedback, is defined in `.ai-context/lifecycle.md`. A feature cannot silently override this constitution; an exception or contradiction requires an explicit governance decision.

## AI Operating Model

### Core principle

AI-first does not mean AI does the thinking. An agent is a literal-minded, context-dependent executor: it performs what is explicitly written and does not reliably infer unstated intent. Precision takes precedence over assumption; verification takes precedence over trust.

### Persistent repository context

- `.ai-context/` is the authoritative persistent project record; chat history is temporary execution context.
- Repeatedly needed knowledge must be captured in an authoritative repository artefact rather than held only in a session.
- Business requirements belong in `.ai-context/BRD.md`; architecture knowledge belongs in `.ai-context/architecture.md`; project-wide rules belong here; current state belongs in `.ai-context/status.md`.
- Significant technical decisions belong in ADRs under `.ai-context/decisions/`; feature intent belongs in specs; technical approach belongs in plans; execution sequence belongs in tasks.
- Link to the authoritative artefact instead of duplicating large blocks of context across files.

### Scoped context protocol

Never provide the entire repository as context for a localized task unless repository-wide analysis is genuinely required. Before work, determine: the task-defining artefact, applicable acceptance criteria, API contract, relevant modules/files, relevant architecture sections, related specs/decisions, and explicit out-of-scope files.

Use this context chain: **Task ID → Spec → Acceptance Criteria → Plan → Relevant Architecture / Related Modules → Only Required Code Context**.

### Stable identity and focused execution

- Prefer prompts using stable identities, for example: `Implement <slug>.T03. Satisfy <slug>.AC3. Follow <slug>.API02.`
- Do not substitute an informal description where stable identifiers are available.
- One task requires one focused prompt: generate, validate, review, then commit.
- Do not silently broaden scope, combine independent tasks, refactor unrelated code, or build an entire feature in one prompt. Flag a task that is too large to execute and review clearly.

### Ambiguity and no-invention rule

When the repository does not define an answer, do not manufacture one. State one of: **Specified**, **Not specified**, **Conflicting**, **Requires decision**, or **TBD**.

For ambiguity, inconsistency, missing decision, or underspecified requirement: identify the affected artefact/task, explain the implementation consequence, and ask for clarification or record an unresolved decision. If an outcome is significantly wrong because a governing artefact is ambiguous, stop and require that artefact to be revised; do not repeatedly prompt around the same ambiguity.

### Model independence and task matching

Model choice is a tooling decision, not an architecture decision. Specifications, plans, tasks, architecture, and this constitution remain governing regardless of the agent or provider used. Do not introduce model-specific business behaviour or architecture unless requirements explicitly require it.

Use lighter/faster models for boilerplate, formatting, regex, simple lint fixes, and mechanical transformations. Use stronger models for architecture reasoning, complex business logic, difficult debugging, cross-module reasoning, significant refactoring, and security-sensitive reasoning. This changes neither requirements nor architecture.

### Session boundaries

Start new agent sessions at feature or bug boundaries. A recommended progression is: discovery/spec, plan, one task per session, then review/validation. Do not depend on a long-running session as project memory.

### Architecture currency

`.ai-context/architecture.md` is a living document. When approved implementation introduces or changes integrations, data stores, service boundaries, important architectural decisions, or significant system behaviour, update the relevant architecture documentation. Stale architecture documentation is unreliable context.

## Testing Discipline

### INT SDD baseline

Tests are derived from approved acceptance criteria and follow test-first practice when implementation is authorized.

Executable tests must be reviewed, run, and confirmed RED before implementation. A passing pre-implementation test is a blocker requiring investigation, not evidence that RED occurred.

### Project-specific rules

TBD — no project-specific testing policy has been supplied.

## Security Posture

### INT SDD baseline

Security-relevant requirements and verification must be explicit and traceable.

### Project-specific rules

TBD — no security strategy, compliance requirement, or threat model has been supplied.

## Architectural Constraints

### INT SDD baseline

Architecture decisions must be documented and approved; unspecified technology or topology must not be inferred.

### Project-specific rules

TBD — no architecture, stack, integration, or data constraints have been supplied.

## Non-Functional Baselines

### INT SDD baseline

Non-functional expectations must be specified and verified where applicable.

### Project-specific rules

TBD — no performance, availability, accessibility, recovery, or compliance targets have been supplied.

## Versioning Rules

### INT SDD baseline

Specifications and delivery artefacts are version-controlled with the repository and are reviewed and diffed like code.

### Project-specific rules

TBD — no product, API, or release versioning policy has been supplied.

## Repository / Engineering Rules

### INT SDD baseline

- Keep repository references portable and relative to the repository root.
- Do not place secrets or sensitive environment values in tracked artefacts.
- Do not implement unapproved feature work.
- Do not use AI attribution in source comments, commits, PR descriptions, or implementation notes; use stable SDD identifiers for traceability.

### Project-specific rules

TBD — no additional repository conventions have been supplied.
