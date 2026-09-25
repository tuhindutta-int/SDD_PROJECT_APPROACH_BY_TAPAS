# INT Specification-Driven Delivery Workspace

This repository follows INT Specification-Driven Delivery (SDD).

The specification is the primary engineering artefact: it is reviewed, versioned, and diffed like code. Code is generated or implemented output derived from approved specifications.

## Delivery Chain

```text
BRD
 ↓
Spec
 ↓
Gate 1
 ↓
Plan
 ↓
Tasks
 ↓
Test-first
 ↓
Implementation
 ↓
Gate 2
 ↓
Merge
 ↓
Release
```

## Workspace

- `.agent/` contains reusable agent rules and workflows that preserve scope, traceability, and explicit decision-making.
- `.ai-context/` contains the project’s business requirements, specifications, plans, tasks, test-case definitions, decisions, and status artefacts.

No feature implementation begins without an approved specification.

## Non-Negotiable Implementation Gates

Normal implementation is blocked unless the approved chain and compliance check are satisfied:

```text
Approved Spec → Approved Plan → Approved Task → Reviewed Test → Confirmed RED → Implementation → GREEN → Human Gate 2
```

The six mandatory controls are defined in `.agent/rules/int-sdd-non-negotiables.md`: approved-spec-only work, no spec-less prompting, test-first RED, no secrets/PII, scoped context, and human accountability for merged lines. Use `.agent/workflows/pre-implementation-compliance.md` before application-code changes.

## Delivery Lifecycle

The governing lifecycle, state transitions, gates, hotfix path, and production feedback loop are documented in `.ai-context/lifecycle.md`. Use `.agent/workflows/sdd-lifecycle-check.md` to validate an action against the current lifecycle state before proceeding.

## Feature Traceability Convention

Each future feature uses a unique kebab-case slug, for example `2fa-otp-login`.

The slug links these future artefacts:

- `.ai-context/specs/<slug>.spec.md`
- `.ai-context/plans/<slug>.plan.md`
- `.ai-context/tasks/<slug>.tasks.md`
- `.ai-context/test_cases/<slug>.test_cases.md`
- `feature/<slug>`

Identifiers use the same slug: `<slug>.AC1`, `<slug>.API01`, `<slug>.UT01`, and `<slug>.T01`.

No example feature artefacts are created during bootstrap.
