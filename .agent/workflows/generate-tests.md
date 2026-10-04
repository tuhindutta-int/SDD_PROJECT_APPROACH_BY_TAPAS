# Generate Tests Workflow

## Authority

This workflow governs test case creation under INT SDD QA governance.

Governing rule: `.agent/rules/int-sdd-qa.md`
QA validation: `.agent/workflows/qa-validation-check.md`
Security checks: `.agent/workflows/security-check.md` Part 1
Test-case template: `.ai-context/test_cases/_template.test_cases.md`
Non-negotiables: `.agent/rules/int-sdd-non-negotiables.md` §3

---

## Purpose

Derive acceptance-oriented test cases and executable tests from an approved
feature specification and authorized plan.

Tests must be derived from what the feature is specified to do — not from
reading the finished implementation.

---

## Inputs Required

- An explicit feature slug.
- Approved `.ai-context/specs/<slug>.spec.md`.
- Approved `.ai-context/plans/<slug>.plan.md` and authorized `.ai-context/tasks/<slug>.tasks.md`.

Do not generate tests before the artefact chain has reached approved tasks.
Tests are downstream of the approved spec, plan, and tasks.
Implementation remains blocked until test-first work is authorized.

---

## Procedure

### Step 1 — Source the spec, not the code

Load the approved spec. Do NOT read implementation code.
The approved spec is the sole source for determining what each test must assert.

If implementation exists before tests are being created:
**STOP** — this is a reversal of the derivation chain. Run
`.agent/workflows/qa-validation-check.md` Part 1 before continuing.

### Step 2 — Map each AC to test coverage

For each acceptance criterion in the spec:

1. Identify the AC ID (`<slug>.ACN`).
2. Determine the observable behaviour specified by the AC.
3. Write the expected result from the specification, not from running the code.
4. Create one or more test cases that cover:
   - The positive path (expected success behaviour)
   - At least one negative path where the AC semantics require it
   - Relevant boundary conditions where the AC specifies limits or thresholds
   - Relevant error conditions specified in the AC or spec

### Step 3 — Assign stable test identities

Assign identities consistent with Line 9 naming governance:

- `<slug>.TCNN` — for QA test-case entries in `test_cases/<slug>.test_cases.md`
- `<slug>.UTNN` — for spec-level executable test references (defined in the spec)

Do not use a test ID already assigned to this feature.
Do not renumber existing IDs.

### Step 4 — Record in the test-case file

For each test case, populate all required fields in the template:
- Test case ID, type, mapped AC, mapped UT, preconditions, steps,
  expected result, pass/fail criterion, negative/boundary notes, QA notes.

### Step 5 — Distinguish AC-derived from broader QA scenarios

Acceptance-derived tests are directly traceable to an AC ID.
Broader QA scenarios (exploratory, data-volume, accessibility) may extend
beyond individual ACs. Label these clearly and do not present them as
new acceptance criteria.

### Step 6 — Security-relevant ACs

For ACs involving authentication, authorization, input validation, secrets
handling, or data protection:
- Include at least one security-relevant negative-path test.
- Note any security test that should be augmented by the security check workflow
  (`.agent/workflows/security-check.md`).

### Step 7 — QA review of generated tests

If tests were AI-generated:
- Run `.agent/workflows/qa-validation-check.md` Part 4.
- QA must verify AC mapping, assertion quality, negative-path coverage.
- Passing AI-generated tests do not constitute QA closure.

### Step 8 — Never alter ACs to make tests pass

Do not change acceptance criteria to make a failing test pass.
If an AC appears untestable: report the ambiguity as a specification issue
(source E) and request resolution through Gate 1 before proceeding.

---

## Completion Check

Before declaring test-case creation complete:

- [ ] Every AC in the spec has at least one test case (TC or UT).
- [ ] Every test has an explicit AC mapping.
- [ ] Every test has observable expected result and pass/fail criteria.
- [ ] Negative and boundary paths are covered where AC semantics require them.
- [ ] Security-relevant ACs have at least one negative-path test.
- [ ] No test was derived from reading finished implementation.
- [ ] No AC was altered to make a test pass.
- [ ] Test IDs are stable and correctly formatted.
- [ ] Tests not derived from a single AC are labelled as broader QA scenarios.
- [ ] Secrets and PII are absent from test fixtures (Part 1 of security-check.md).
- [ ] Run `.agent/workflows/qa-validation-check.md` Parts 1–2 and confirm passed.
