# Code Review Workflow

## Purpose

Review an implementation against its approved specification, plan, tasks, and acceptance criteria.

## Inputs

- Explicit feature slug and implementation change set.
- Approved spec, plan, tasks, and test cases for that slug.

## Procedure

1. Confirm the task ID, related spec, applicable acceptance criteria, API contract, and approved lifecycle state before reviewing.
2. Check every change against approved plan approach, task scope boundaries, and acceptance criteria.
3. Verify architecture constraints, security requirements, and non-functional constraints stated in the constitution and spec.
4. Verify test-first evidence, test results, and that acceptance-derived tests map to AC IDs.
5. Check relevant documentation updates, including `architecture.md` for integrations, data stores, service boundaries, important decisions, or significant system behaviour.
6. Identify scope expansion, plan/task divergence, missing traceability, regressions, unverified behaviour, and unrelated changes.
7. Do not approve inferred requirements or undocumented architecture changes.
8. Report ambiguity and required specification or decision updates instead of guessing.
9. Confirm human accountability: Gate 2 review and merge approval cannot be replaced by AI generation, AI attribution, or passing tests.

## Completion Check

Provide findings tied to artefact IDs and acceptance criteria, with a clear Gate 2 outcome or required changes.
