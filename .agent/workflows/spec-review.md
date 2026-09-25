# Specification Review Workflow

## Purpose

Validate a feature specification before it enters Gate 1. The authoritative standard is `.agent/rules/int-sdd-specification-standard.md`.

## Inputs

- `.ai-context/specs/<feature-slug>.spec.md`
- Linked `.ai-context/BRD.md#BRD-NNN`
- Applicable constitution, architecture, ADRs, and related specs.

## Review Checks

1. **One feature / identity:** bounded single feature; unique specific kebab-case slug.
2. **BRD linkage:** linked business requirement exists and feature intent does not diverge from it.
3. **Intent:** one unambiguous behaviour paragraph stating what changes, for whom, when, and outcome.
4. **Acceptance criteria:** stable AC IDs; objective, observable, testable behaviour; Given/When/Then where applicable.
5. **Failure paths:** applicable invalid, unauthorized, missing, boundary, duplicate, partial, timeout, state, and error behaviour is defined or explicitly flagged.
6. **API contract:** when an API is exposed or consumed, stable endpoint IDs, method/path, request, success status/body, errors, and validation behaviour are complete; otherwise the spec states Not applicable.
7. **Test cases:** stable UT IDs map to AC IDs; acceptance-level cases are distinct from broader QA.
8. **Scope / constraints:** explicit out-of-scope items; applicable constitution-derived non-functional constraints.
9. **Context / dependencies:** references are real, relevant, and have usable status; no broad-context duplication.
10. **Alignment:** constitution, architecture, and ADR constraints are represented; required stakeholder/sign-offs and open decisions are visible.
11. **Safety:** no secrets, PII, production data, or confidential data.
12. **Boundary:** no plan, task, or speculative implementation detail masquerades as feature intent.

## Outcome

- **READY FOR GATE 1:** every required check passes; list the evidence and remaining explicitly accepted non-blocking TBD items, if any.
- **CHANGES REQUIRED:** list each blocking issue with affected section, reason, impact, and required correction.

Codex must not approve its own specification. A readiness result is not Gate 1 approval; approval requires the recorded named human non-author review defined by the lifecycle.
