# Generate Tests Workflow

## Purpose

Derive acceptance-oriented test cases and executable tests from an approved feature specification and authorized plan.

## Inputs

- An explicit feature slug.
- Approved `.ai-context/specs/<slug>.spec.md`.
- Approved `.ai-context/plans/<slug>.plan.md` and authorized `.ai-context/tasks/<slug>.tasks.md`.

## Procedure

1. Map each acceptance-derived test to a declared acceptance criterion using the feature identifier convention.
2. Define observable preconditions, action, and expected outcome.
3. Distinguish acceptance-derived tests from broader QA scenarios; broader QA must not be presented as new acceptance criteria.
4. Cover specified validation, error, boundary, and security behaviours where the specification requires them.
5. Create reviewed executable tests before implementation, run them, and record confirmed RED evidence before any implementation work; work in small increments only when implementation is authorized.
6. Never alter acceptance criteria merely to make a test pass.
7. Do not create tests for inferred behaviours or unstated technical choices.
8. Report ambiguities or untestable acceptance criteria for specification clarification.

Do not use this workflow before the artefact chain has reached approved tasks. Tests are downstream of the approved spec, plan, and tasks; implementation remains blocked until test-first work is authorized. If a new test passes before implementation, stop and investigate rather than claiming RED.

## Completion Check

Verify traceability from each test to acceptance criteria and report any uncovered criteria.
