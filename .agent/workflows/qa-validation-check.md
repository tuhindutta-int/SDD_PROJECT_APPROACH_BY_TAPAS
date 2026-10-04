# QA Validation Check Workflow

## Authority

This is the QA validation workflow for INT SDD Line 11.

Governing rule: `.agent/rules/int-sdd-qa.md`
Security validation: `.agent/workflows/security-check.md`
Non-negotiables: `.agent/rules/int-sdd-non-negotiables.md`
Generate-tests workflow: `.agent/workflows/generate-tests.md`
Test-case template: `.ai-context/test_cases/_template.test_cases.md`
Lifecycle: `.ai-context/lifecycle.md`

---

## When to Run

- **Part 1** (Test Derivation Check): Before test cases are created, to verify the source chain.
- **Part 2** (Test Case Coverage): After test cases are written, before RED runs.
- **Part 3** (Test-First Evidence): Before implementation is authorized for a task.
- **Part 4** (QA Review of AI-Generated Tests): When tests are AI-generated.
- **Part 5** (UAT Traceability): During UAT or when reviewing UAT outcomes.
- **Part 6** (Integration and Regression Coverage): When cross-feature scenarios are involved.
- **Part 7** (Gate 2 QA Evidence): As part of Gate 2 preparation.
- **Full check**: When performing a QA audit of a feature in or near QA state.

---

## OUTPUT FORMAT

Produce one of:

```
QA CHECK PASSED
```

```
QA CHECK — BLOCKING: <findings>
QA CHECK — WARNING: <findings>
```

Use **BLOCKING** for issues that prevent the current step from proceeding.
Use **WARNING** for issues requiring human attention that do not halt the step.

---

## Part 1 — Test Derivation Check

_Run before test cases are created._

### 1.1 Source chain

- [ ] **[BLOCKING]** Approved spec exists: `.ai-context/specs/<feature-slug>.spec.md`
- [ ] **[BLOCKING]** Spec status is `Approved`; Gate 1 completed with named reviewer
- [ ] **[BLOCKING]** Acceptance criteria are defined and have stable IDs (`<slug>.ACN`)
- [ ] **[BLOCKING]** Approved plan exists: `.ai-context/plans/<feature-slug>.plan.md`
- [ ] **[BLOCKING]** Approved tasks exist: `.ai-context/tasks/<feature-slug>.tasks.md`
- [ ] **[BLOCKING]** Test cases are being derived from the approved spec, not from
      reading the implementation

### 1.2 Derivation source verification

For each test case being created, verify:

- [ ] **[BLOCKING]** The tester can identify the AC from which the test is derived
      before reading any implementation code.
- [ ] **[BLOCKING]** The expected result is determined from the spec, not from
      running the code and observing what happens.
- [ ] **[WARNING]** Implementation code has not been read prior to writing the
      test (test-first practice).

**Part 1 result:**

```
DERIVATION SOURCE: Approved spec / Unapproved or reversed
```

---

## Part 2 — Test Case Coverage Check

_Run after test cases are written, before RED runs._

### 2.1 AC coverage matrix

For each AC in the approved spec:

| AC ID | Covered by (TC/UT IDs) | Coverage status | Notes |
| ----- | ---------------------- | --------------- | ----- |
| `<slug>.AC1` | `<slug>.TC01, .UT01` | `Covered / Partial / Not covered` | |

- [ ] **[BLOCKING]** Every AC that requires verification has at least one
      corresponding test case (`TC` or `UT`).
- [ ] **[WARNING]** Security-relevant ACs have at least one negative-path test.
- [ ] **[WARNING]** Boundary-condition ACs have boundary test coverage.

### 2.2 Test case identity validation

For each test case:

- [ ] **[BLOCKING]** Test case ID follows the format `<slug>.TCNN` or `<slug>.UTNN`.
- [ ] **[BLOCKING]** Test case ID has not been used before for this feature
      (no duplicates).
- [ ] **[BLOCKING]** Test case ID prefix matches the feature slug exactly.
- [ ] **[BLOCKING]** Each test references a valid, non-retired AC ID.
- [ ] **[WARNING]** No test references an AC from a different feature without
      explicit cross-feature governance.

### 2.3 Test quality review

For each test case:

- [ ] **[BLOCKING]** Test has a clearly observable expected result.
- [ ] **[BLOCKING]** Test has explicit pass/fail criteria (not "looks correct").
- [ ] **[WARNING]** Test has at least one negative or boundary-condition scenario
      where the AC semantics require it.
- [ ] **[WARNING]** Test cannot trivially pass without asserting meaningful behaviour.
- [ ] **[BLOCKING]** Test preconditions are explicitly stated.

### 2.4 Orphaned and stale tests

- [ ] **[BLOCKING]** No test references a retired AC ID.
- [ ] **[BLOCKING]** No test references a non-existent AC ID.
- [ ] **[WARNING]** Tests from previous iterations that reference changed ACs have
      been reviewed and updated or retired.

**Part 2 result:**

```
COVERAGE STATUS: <feature-slug>
ACs covered:            <N>
ACs with no coverage:   <N> — [list]
Orphaned tests:         <N> — [list]
```

---

## Part 3 — Test-First Evidence Check

_Run before implementation is authorized for each task._

For each task being implemented:

- [ ] **[BLOCKING]** Test cases covering this task's AC IDs exist in the test-case file.
- [ ] **[BLOCKING]** Executable tests for this task's ACs are created and reviewed.
- [ ] **[BLOCKING]** RED evidence is confirmed: the test was run and observed to fail
      before implementation. Evidence must include:
      - Test command or run method
      - Failing output (error/assertion failure message)
      - Reason the test fails (not: "test failed as expected" without evidence)
- [ ] **[BLOCKING]** RED evidence was produced without first reading or writing
      the implementation.

**Part 3 result:**

```
TEST-FIRST STATUS: <slug>.TNN
RED confirmed: Yes / No
RED evidence: [recorded / missing]
```

---

## Part 4 — AI-Generated Test Review

_Run when tests have been generated by an AI agent._

### 4.1 Generated test quality

For each AI-generated test:

- [ ] **[BLOCKING]** Test explicitly maps to a specific AC ID (`<slug>.ACN`).
- [ ] **[BLOCKING]** Test asserts specified behaviour from the AC, not
      implementation structure.
- [ ] **[BLOCKING]** Test can produce a meaningful failure (not a tautology).
- [ ] **[WARNING]** Test includes at least one negative or error path where the
      AC semantics require it.
- [ ] **[WARNING]** Test covers at least one boundary condition where applicable.
- [ ] **[WARNING]** Test does not duplicate another test scenario for the same AC.
- [ ] **[WARNING]** Test would fail against a plausible incorrect implementation
      of the same AC.

### 4.2 AI-generated test completeness review

QA must evaluate whether AI-generated tests adequately cover:

| Area | QA check required |
| ---- | ----------------- |
| All AC positive paths | Yes |
| All AC negative paths | Yes — not assumed covered by AI |
| All AC boundary conditions | Yes |
| Data-volume variation | Human judgement required |
| Device/browser matrix (where applicable) | Human judgement required |
| Accessibility (where applicable) | Human judgement required |
| Grey-area interpretation | Human judgement required |
| Cross-feature regression | Human judgement required |

### 4.3 AI-test prohibited conclusions

The following conclusions are explicitly NON-COMPLIANT:

- "All generated tests pass, therefore QA is complete." → **NON-COMPLIANT**
- "AI generated the tests, so no QA review is needed." → **NON-COMPLIANT**
- "Tests pass, so the specification is complete." → **NON-COMPLIANT**

**Part 4 result:**

```
AI-GENERATED TEST REVIEW: <feature-slug>
Tests reviewed: <N>
Tests compliant: <N>
Tests requiring revision: <N> — [list]
QA additions required: [list or None]
```

---

## Part 5 — UAT Traceability Check

_Run during UAT or when reviewing UAT outcomes._

### 5.1 UAT scenario traceability

For each UAT scenario:

- [ ] **[BLOCKING]** UAT scenario traces to one or more `<slug>.ACN` IDs.
- [ ] **[WARNING]** UAT scenario without a matching AC — investigate whether this
      represents a specification gap.

### 5.2 UAT failure classification

For each UAT failure:

Determine the failure type:

| Type | Condition | Action |
| ---- | --------- | ------ |
| **Implementation failure** | Required behaviour is in the AC; code fails to produce it | Fix implementation (classify as B) |
| **Test/scenario failure** | UAT scenario is incorrect or misconfigured | Fix the UAT scenario (classify as A) |
| **Specification gap** | Required behaviour has no matching AC | Investigate spec; update through Gate 1 if behaviour is required |

- [ ] **[BLOCKING]** Every UAT failure has been classified before code is changed.
- [ ] **[BLOCKING]** Specification gaps identified by UAT are routed to the spec
      through the correct governance process, not silently implemented.
- [ ] **[BLOCKING]** No code change was made to satisfy UAT without the spec being
      updated where the behaviour was not previously specified.

**Part 5 result:**

```
UAT TRACEABILITY: <feature-slug>
Scenarios with AC trace: <N>
Scenarios without AC (specification gap candidates): <N> — [list]
Failures classified: Yes / No
```

---

## Part 6 — Integration and Regression Coverage

_Run when cross-feature scenarios are relevant._

### 6.1 Integration test coverage

- [ ] **[WARNING if missing]** Cross-feature interactions introduced by this feature
      are documented in `.ai-context/test_cases/_integration.md`.
- [ ] **[WARNING]** Integration scenarios clearly identify which features/specs they span.
- [ ] **[BLOCKING]** Integration scenarios do not duplicate feature-level test
      cases without justification.

### 6.2 Regression impact

When a spec or implementation changes:

- [ ] **[WARNING if not done]** Existing TC/UT IDs that may be affected by the change
      have been identified and reviewed.
- [ ] **[WARNING if not done]** Cross-feature scenarios in `_integration.md` that may
      be affected have been identified and reviewed.
- [ ] **[WARNING]** Regression scope is explicitly documented before implementation
      restarts after a spec change.

**Part 6 result:**

```
INTEGRATION/REGRESSION COVERAGE: <feature-slug>
New cross-feature scenarios documented: Yes / No / Not applicable
Regression impact reviewed: Yes / No / Not applicable
```

---

## Part 7 — Gate 2 QA Evidence Check

_Run as part of Gate 2 preparation._

### 7.1 QA evidence required for Gate 2

- [ ] **[BLOCKING]** AC coverage matrix is complete: every verifiable AC has
      documented test coverage.
- [ ] **[BLOCKING]** Test-first RED evidence exists for all tasks in the feature.
- [ ] **[BLOCKING]** Test-first GREEN evidence exists for all tasks in the feature.
- [ ] **[BLOCKING]** AI-generated tests have been QA-reviewed.
- [ ] **[BLOCKING]** No test references a retired or non-existent AC.
- [ ] **[BLOCKING]** UAT scenarios (where applicable) trace to ACs.
- [ ] **[BLOCKING]** UAT failures (if any) have been resolved through the correct
      governance path.
- [ ] **[WARNING]** QA has explicitly evaluated coverage for negative paths,
      boundary conditions, and human-judgement areas.
- [ ] **[WARNING]** Integration/regression coverage has been considered.

### 7.2 QA evidence record format

Include in the Gate 2 evidence record:

```
Feature slug:           <slug>
AC coverage:            All covered / <N> uncovered [list]
Test IDs:               [list of TC/UT IDs]
RED evidence:           Confirmed for tasks [list]
GREEN evidence:         Confirmed for tasks [list]
AI test review:         Completed / Not completed
UAT traceability:       Confirmed / N/A / Gaps found [list]
Integration coverage:   Documented / N/A
QA reviewer/owner:      <name or role>
Outstanding QA issues:  [list or None]
Date:                   YYYY-MM-DD
```

**Part 7 result:**

```
GATE 2 QA EVIDENCE: <feature-slug>
Status: Ready / Not ready
Blocking items: [list or None]
```

---

## Findings Format

For each finding:

```
ID:                  QA-<AREA>-NNN
Severity:            BLOCKING | WARNING
Area:                <area code>
Observation:         <what was found>
Why it matters:      <what QA principle is violated>
Evidence:            <specific test ID, AC ID, or absent item>
Required action:     <what must be done>
```

Area codes:

| Code | Area |
| ---- | ---- |
| DRV | Test derivation source |
| COV | AC coverage |
| IDN | Test case identity |
| QAL | Test quality |
| ORP | Orphaned/stale test |
| RED | Test-first RED evidence |
| GEN | AI-generated test review |
| UAT | UAT traceability |
| INT | Integration/regression |
| EVD | Gate 2 QA evidence |

---

## Scenario Quick Reference

| Scenario | Expected result |
| -------- | --------------- |
| A: Test written from inspecting finished code | Part 1 — DRV — BLOCKING |
| B: AI generates tests from approved ACs before implementation | Compliant starting point; run Part 4 |
| C: All generated tests pass; QA finds missing edge scenario | Part 4 — GEN — QA may add cases; not QA closed |
| D: UAT finds required behaviour with no matching AC | Part 5 — UAT — specification gap investigation |
| F: Dependency suggested by AI automatically installed | Security check — not QA check; see security-check.md |
| O: Test fails because spec doesn't define expected edge behaviour | Part 5 — specification gap; do not patch code |
