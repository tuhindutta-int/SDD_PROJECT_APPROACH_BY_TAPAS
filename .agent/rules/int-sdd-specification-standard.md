# INT SDD Specification Standard

## Authority

This is the authoritative repository rule for feature-specification quality before Gate 1. One feature equals one specification. A spec defines feature intent and correctness; it is not a plan, coding prompt, task list, architecture diary, or repository-context dump.

## Feature Boundary and Identity

- One spec represents one bounded, independently reviewable and deliverable feature. If a request contains multiple independent features, stop and identify the required decomposition.
- A spec ID is a unique, human-readable kebab-case `<feature-slug>`, normally three to five specific words. Do not use generic IDs such as `feature-42`, `misc-change`, or `new-feature`, and never silently reuse or change a slug once downstream artefacts exist.
- The slug links the spec, plan, tasks, test cases, branch, reviews, status, and release records.

## Pre-Writing Gate

Before drafting or revising a spec, establish: one unambiguous intent paragraph; linked BRD entry; relevant specs and dependencies; applicable architecture/design constraints; API contract information if an API surface exists; required stakeholders/sign-offs; and explicitly identified unresolved decisions.

If a critical item is unknown, do not finalize the spec. State the missing item as `Not specified`, `Conflicting`, `Requires decision`, or `TBD`. The business requirement must first be captured in `.ai-context/BRD.md`; the next action for a missing requirement is Discovery / BRD, not spec authoring.

## Required Contract Content

Use the content order in `.ai-context/specs/_template.spec.md`: identity, status/revision, linked BRD, intent, context/references, conditional API contract, acceptance criteria, spec-derived unit test cases, explicitly out of scope, non-functional constraints, and open decisions.

- Intent is one concise paragraph stating what changes, for whom, under what condition, and expected outcome.
- Context links authoritative artefacts rather than duplicating broad repository context. Referenced related specs and dependencies must exist, be relevant, and have usable status.
- Do not put framework choices, class names, database structures, internal algorithms, source layout, library selection, ownership mechanics, or call sequences in a spec unless they are already-approved contractual constraints.

## Acceptance Criteria and Testability

- Each AC has a stable `<feature-slug>.ACN` ID, is explicit, behaviour-oriented, objectively testable, and uses Given/When/Then where applicable.
- Each AC must identify observable evidence of satisfaction; vague claims such as “secure,” “fast,” “robust,” or “works correctly” fail review.
- Include applicable failure paths: invalid input, authorization/authentication failure, missing data, validation, boundaries, duplicates, partial failures, timeouts, state transitions, and error responses. If required behaviour is unknown, flag it; do not assume it.
- The spec includes acceptance-level test cases with stable `<feature-slug>.UTNN` IDs mapped to AC IDs. Broader QA belongs in `.ai-context/test_cases/<feature-slug>.test_cases.md` and remains traceable to the spec.

## API Contract

When the feature exposes or consumes an API, the spec must define each `<feature-slug>.APINN` contract: method, path or operation, request shape, successful response shape and status, exception conditions, error responses, and relevant validation behaviour. The contract must be sufficient to test associated ACs.

When no API surface exists, state `Not applicable — no API surface is specified.` Do not invent an API merely to complete a template. Product-level API behaviour belongs in the spec; implementation mechanics belong in the plan.

## Scope, Constraints, and Safety

- Explicitly state excluded adjacent behaviours. Implicit exclusions are insufficient.
- Include applicable non-functional constraints only when sourced from the constitution or approved decisions. Do not invent targets or use vague aspirations.
- Do not include secrets, PII, credentials, production data, or confidential data. Use synthetic placeholders.

## Gate 1 Readiness and Change Control

Run `.agent/workflows/spec-review.md` before entering Gate 1. A spec may enter peer review only when all required checks pass; Codex may report readiness but cannot approve its own spec. Gate 1 approval remains a named human, non-author governance event.

When a spec changes after planning begins, identify downstream impact on the plan, tasks, test cases, executable tests, and implementation. If behaviour changes materially, stop and regenerate or review affected downstream artefacts. Do not silently keep stale downstream work or edit a spec to match existing code.

## Anti-Patterns to Block

Block giant system specs, vague/non-testable ACs, requirements hidden in technical prose, implementation instructions disguised as requirements, missing scope boundaries, referenced-but-undefined APIs, happy-path-only contracts, missing errors, duplicated repository context, invented requirements, unresolved decisions written as facts, bundled independent features, copied implementation details, and specs altered merely to fit current code.
