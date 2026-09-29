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

## Specification Standard

One feature, one specification. A specification is a feature contract that defines what is built and what correctness means — not how it is built.

Three-stage specification governance:

1. **Pre-writing gate:** `.agent/workflows/spec-prewriting-gate.md` — run before drafting a spec.
2. **Quality standard:** `.agent/rules/int-sdd-specification-standard.md` — the authoritative governing rules.
3. **Review workflow:** `.agent/workflows/spec-review.md` — run before Gate 1.

Template: `.ai-context/specs/_template.spec.md`.

No spec may self-approve. Gate 1 requires a named human non-author review.

## Gate 1 Peer Review

Gate 1 is the mandatory human peer review of the specification before implementation begins. It is pre-code design and intent validation — not post-code bug finding.

- **Gate 1 rule:** `.agent/rules/int-sdd-gate-1.md` — governing principles, reviewer requirements, and review dimensions.
- **Gate 1 workflow:** `.agent/workflows/gate-1-review.md` — the reviewer-facing checklist and structured report format.

Gate 1 evaluates: ambiguity, testability, scope, API contract, constitution compliance, overlap, and dependencies.
Gate 1 has two outcomes only: `Approved` or `Changes Requested`.
AI may assist; AI may not approve. The reviewer must be a named human who is not the spec author.

## Architecture and Repository Standards

Every plan is checked against the constitution and architecture before task generation.

- **Architecture rule:** `.agent/rules/int-sdd-architecture.md` — governing principles for plans, ADRs, architecture.md, and repository practices.
- **Architecture check:** `.agent/workflows/architecture-check.md` — run after plan is drafted, before task generation.
- **ADR assessment:** `.agent/workflows/adr-check.md` — determines whether a significant decision requires an ADR.
- **Repository check:** `.agent/workflows/repository-check.md` — verifies branch naming, scope, PR traceability, and commit compliance.
- **ADR template:** `.ai-context/decisions/_template.adr.md` — canonical ADR structure.
- **ADR store:** `.ai-context/decisions/` — project-global ADR files (`ADR-NNNN-<slug>.md`).

Key rules: one feature branch per spec, slug-traceable branches, squash-merge to main, no AI attribution in commits, task-ID traceability in commits.

## Non-Negotiable Implementation Gates

Normal implementation is blocked unless the approved chain and compliance check are satisfied:

```text
Approved Spec → Approved Plan → Approved Task → Reviewed Test → Confirmed RED → Implementation → GREEN → Human Gate 2
```

The six mandatory controls are defined in `.agent/rules/int-sdd-non-negotiables.md`: approved-spec-only work, no spec-less prompting, test-first RED, no secrets/PII, scoped context, and human accountability for merged lines. Use `.agent/workflows/pre-implementation-compliance.md` before application-code changes.

## Delivery Lifecycle

The governing lifecycle, state transitions, gates, hotfix path, and production feedback loop are documented in `.ai-context/lifecycle.md`. Use `.agent/workflows/sdd-lifecycle-check.md` to validate an action against the current lifecycle state before proceeding.

## Naming and Identifier Governance (Line 9)

The feature slug is the canonical root identity for a feature — not a cosmetic filename.
It is assigned at spec creation, remains stable for the feature's lifetime, and must
never be reused after retirement.

- **Naming rule:** `.agent/rules/int-sdd-naming.md` — slug format, immutability, archival, prompt-by-identity, ID hierarchy.
- **Naming check workflow:** `.agent/workflows/naming-identifier-check.md` — validates slug, IDs, cross-references, and collision.
- **Slug registry:** `.ai-context/slug-registry.md` — authoritative record of all slugs (active, retired, archived).

### Traceability chain

```
BRD-NNN
    ↓
<feature-slug>          ← slug assigned at spec creation
    │
    ├── .ai-context/specs/<slug>.spec.md
    │       └── <slug>.AC1, .API01, .UT01
    │
    ├── .ai-context/plans/<slug>.plan.md
    ├── .ai-context/tasks/<slug>.tasks.md       → <slug>.T01, .T02 …
    ├── .ai-context/test_cases/<slug>.test_cases.md
    │
    ├── feature/<slug>           (branch)
    ├── [<slug>] description     (PR)
    └── ADR-NNNN                 (project-global; not slug-scoped)
```

### Stable ID hierarchy

| ID type | Format | Scoped to |
| ------- | ------ | --------- |
| Acceptance criteria | `<slug>.AC1` … | Feature |
| API contract entries | `<slug>.API01` … | Feature |
| Unit test cases | `<slug>.UT01` … | Feature |
| Tasks | `<slug>.T01` … | Feature |
| Business requirements | `BRD-NNN` | Project |
| Architectural decisions | `ADR-NNNN` | Project-global |

IDs are stable identities, not positional labels. Reordering artefact items
does not shift IDs. Retired IDs are never reassigned.

No example feature artefacts are created during bootstrap.
