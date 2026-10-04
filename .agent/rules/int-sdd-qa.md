# INT SDD QA Governance

## Authority

This is the authoritative repository rule for INT SDD Line 11 QA Governance.
It defines how quality assurance is performed, structured, and validated within
the SDD delivery model.

Related artefacts:
- QA validation workflow: `.agent/workflows/qa-validation-check.md`
- Security governance: `.agent/rules/int-sdd-security.md`
- Security check workflow: `.agent/workflows/security-check.md`
- Non-negotiables (test-first): `.agent/rules/int-sdd-non-negotiables.md` §3
- Development discipline: `.agent/rules/int-sdd-development.md`
- Generate tests workflow: `.agent/workflows/generate-tests.md`
- Test-case template: `.ai-context/test_cases/_template.test_cases.md`
- Integration test cases: `.ai-context/test_cases/_integration.md`
- Lifecycle: `.ai-context/lifecycle.md`
- Constitution: `.ai-context/constitution.md`

---

## 1. The QA Source-of-Truth Chain

QA derives from what the feature is specified to do.

```
Acceptance Criteria (approved spec)
        ↓
Spec-derived Test Cases
        ↓
Test-first / RED (confirmed before implementation)
        ↓
Implementation
        ↓
GREEN (implementation satisfies the defined test)
        ↓
QA judgement
        ↓
UAT / regression where applicable
```

**Tests must be derived from WHAT THE FEATURE IS SUPPOSED TO DO.**

Not:

```
Implementation → infer behaviour from code → write tests around implementation
```

A test that merely verifies how code happens to be written is not a valid
specification-level test. A test that passes against incorrect implementation
is not proof of correctness.

This is the most fundamental QA principle. It must never be reversed.

---

## 2. Tests Derive from Acceptance Criteria, Not Finished Code

**This is a blocking governance rule.**

The mandatory test-generation sequence is:

```
Approved Spec → ACs → spec-derived test cases → QA review
→ test-first / RED → implementation → GREEN
```

NOT:

```
Implementation → read source code → infer behaviour → write tests → declare success
```

When test cases are being created:

- [ ] Load the approved spec, not the implementation.
- [ ] Derive test scenarios from each acceptance criterion.
- [ ] Define expected outcomes from the spec, not from running the code.
- [ ] Do not read implementation to determine what to assert.

If no approved spec or AC exists, test-case creation must wait.
Do not generate tests against undefined behaviour.

**Violation indicator:** Test scenarios that can only be known by reading the
finished code, that assert implementation structure rather than specified
behaviour, or that were written after GREEN was already confirmed.

---

## 3. Acceptance Criteria → Test Case Traceability

### 3.1 Mapping model

Every acceptance criterion that requires verification must be traceable to one
or more test cases.

```
<feature-slug>.AC1
    ↓
<feature-slug>.TC01  (QA test case in test_cases/<slug>.test_cases.md)
<feature-slug>.UT01  (executable unit test, referenced in spec)
```

One AC can map to multiple tests.
One test can address multiple related ACs where semantically appropriate.
A test without a valid, non-retired AC mapping is an orphaned test — flag it.

### 3.2 Test identity namespaces

The repository uses two distinct test-identity namespaces:

| Namespace | Format | Defined in | Purpose |
| --------- | ------ | ---------- | ------- |
| **TC** | `<slug>.TCNN` | `.ai-context/test_cases/<slug>.test_cases.md` | QA test-case identity — broader validation scenarios including negative, boundary, manual, and integration paths |
| **UT** | `<slug>.UTNN` | `.ai-context/specs/<slug>.spec.md` | Spec-level unit-test identity — maps to executable test functions; defined at spec authoring |

These are distinct identities. Do not collapse them.

- `UT` is defined in the spec. It links a specific spec-level test expectation to implementation.
- `TC` is defined in the test-case file. It documents the full QA test scenario, which may wrap one or more `UT` references.

A `TC` references its corresponding `UT` where one exists. A `TC` may exist
without a `UT` for manual, exploratory, or integration-level scenarios.

### 3.3 Traceability answers

| Question | Answer |
| -------- | ------ |
| Can one AC map to multiple tests? | Yes — both TC and UT levels |
| Can one test cover multiple ACs? | Yes — when semantically appropriate; document explicitly |
| Can a test exist without an AC? | No for spec-derived tests. Yes for integration/regression scenarios — document intent |
| What happens when an AC changes? | The AC update propagates: linked TCs and UTs must be reviewed and updated or retired |
| What happens when a test becomes stale? | Retire the test ID; do not renumber; flag orphaned AC coverage |
| How are orphaned tests detected? | Run `.agent/workflows/qa-validation-check.md`; any TC/UT referencing a retired or non-existent AC is orphaned |

---

## 4. Test-Case Ownership and Structure

### 4.1 Per-feature test cases

Location: `.ai-context/test_cases/<feature-slug>.test_cases.md`

Contains:
- Specification-derived acceptance-level scenarios (mapped to AC IDs)
- Expanded negative path scenarios
- Boundary and data-variation scenarios
- Integration scenarios owned by this feature
- Expected results with explicit pass/fail criteria
- RED/GREEN confirmation fields
- QA notes and risk observations

See template: `.ai-context/test_cases/_template.test_cases.md`

### 4.2 Integration and system-level test cases

Location: `.ai-context/test_cases/_integration.md`

Contains:
- Cross-feature journeys and workflows
- System-level scenarios spanning multiple specs
- Regression suites
- Cross-feature negative/boundary paths
- Scenarios that cannot be cleanly owned by one feature

**Do NOT** duplicate every feature's unit tests in `_integration.md`.
**Do NOT** turn `_integration.md` into a catch-all for untraceable tests.

The purpose is:

```
Feature-local traceability (per-feature test_cases file)
+
System-level and cross-feature coverage (_integration.md)
```

Each must complement, not duplicate, the other.

### 4.3 Test-case fields (required per test case)

Every documented test case must include:

| Field | Requirement |
| ----- | ----------- |
| Test case ID | `<slug>.TCNN` — stable, sequential, never reused |
| Type | `Unit / Integration / E2E / Manual / UAT` |
| Maps to AC | One or more valid, non-retired `<slug>.ACN` |
| Maps to UT | `<slug>.UTNN` where an executable test exists |
| Preconditions | Explicit system state before test |
| Test steps | Numbered, reproducible steps |
| Expected result | Explicit observable outcome from the spec |
| Pass/fail criterion | How pass or fail is objectively determined |
| Negative/boundary notes | Relevant failure paths or boundary conditions |
| RED confirmed | `Yes / No / Not yet run` |
| GREEN confirmed | `Yes / No / Not yet run` |
| QA notes | Risk observations, grey-area flags, QA judgement record |

---

## 5. AI-Generated Tests — Governance

### 5.1 AI-generated tests are a starting point

AI-generated tests are NOT:
- QA approval
- evidence that the specification is complete
- proof of production correctness
- a replacement for QA judgement

They are a starting point for QA review and augmentation.

### 5.2 QA must review generated tests for

- Correct mapping to the specified AC (not to inferred code behaviour)
- Meaningful assertions (not tautologies or empty stubs)
- Negative-path coverage
- Boundary-condition coverage
- Absence of implementation-coupled assumptions
- Absence of duplicated scenarios
- Coverage of specified error conditions
- Independent reproducibility

### 5.3 What "all generated tests pass" does NOT mean

- It does not mean QA is satisfied.
- It does not mean the test suite fully captures the intended requirement.
- It does not mean the feature is ready for release.
- It does not replace QA judgement, exploratory testing, or UAT.

GREEN proves the implementation satisfies the executed test.
It does NOT prove the test suite fully captures the intended requirement.

### 5.4 Prohibited AI-test patterns

| Pattern | Why it is prohibited |
| ------- | -------------------- |
| Test written by inspecting finished code | Reverses the derivation chain |
| Test asserts implementation structure, not behaviour | Not a specification-level test |
| All tests generated without QA review | AI generation ≠ QA approval |
| AC mapping omitted from generated test | Orphaned test; not traceable |
| Test modified to match code rather than spec | Weakens specification contract |

---

## 6. QA Judgement Boundary

### 6.1 Where QA judgement is required

Human QA judgement is required in areas that automated test generation cannot
reliably cover:

| Area | Why human judgement is required |
| ---- | ------------------------------- |
| Data-volume variation | Generated tests cannot determine real production data patterns |
| Device and browser matrix | Requires project-specific compatibility decisions |
| Accessibility | Requires human and/or specialist tooling judgement |
| Exploratory validation | Requires reasoning about unspecified edge behaviour |
| Grey-area requirement interpretation | Requires QA to flag missing or ambiguous ACs |
| Cross-feature regression | Requires system-level understanding of inter-feature risk |
| Real-world workflow judgement | Requires understanding how users actually use the system |
| Risk-based test prioritization | Requires QA to assess likelihood and impact |
| Usability and UX validation | Not derivable from ACs alone |

### 6.2 Applicability principle

Not every QA area applies to every task. Use risk-based applicability.

- A backend-only task with no UI may not require device/browser matrix testing.
- A feature with no accessibility impact may not require accessibility review.
- A read-only feature may not require write-path regression.

Explicitly state `Not applicable — <reason>` rather than silently omitting checks.

---

## 7. UAT Traceability

### 7.1 UAT must trace to AC IDs

UAT scenarios must be traceable back to acceptance criteria by stable ID.

```
UAT-<identifier> (or QA-scenario reference)
    ↓
<feature-slug>.AC1
```

Where a UAT framework uses explicit scenario IDs, those IDs must link to the
relevant AC in the approved spec.

### 7.2 UAT failure with no matching AC — specification gap protocol

A UAT failure that has no matching acceptance criterion is a signal that:
- The specification may be incomplete.
- Gate 1 may have missed a requirement.
- An unstated business requirement is being tested.

The default response is NOT:

```
"Just change the code until UAT passes."
```

The default response IS:

```
UAT gap identified
    ↓
Investigate whether the behaviour is truly required
    ↓
If required: determine whether the AC is missing or incorrect
    ↓
Update the authoritative artefact (spec) through the correct process
    ↓
Propagate downstream artefact changes
    ↓
Regenerate/update tests as required
    ↓
Retest
```

Do not silently implement behaviour to satisfy UAT without updating the spec.
New intended behaviour must be captured in the governing artefact.

### 7.3 UAT traceability at Gate 2

Gate 2 must confirm that:
- UAT scenarios can be traced to AC IDs where applicable.
- UAT failures have been investigated and resolved through the correct governance path.
- No UAT-driven code changes bypassed the specification update process.

---

## 8. Test Coverage Gap Detection

The following must be detected and flagged:

| Gap type | Severity |
| -------- | -------- |
| AC with no test coverage | WARNING or BLOCKING (depending on risk) |
| Test referencing retired or non-existent AC | BLOCKING |
| Duplicate test ID | BLOCKING |
| Test derived from implementation rather than AC | BLOCKING |
| Missing negative-path coverage where AC semantics require it | WARNING |
| Unresolved grey-area requirement with no QA note | WARNING |
| Test that cannot produce meaningful failure evidence | WARNING |
| Test that passes trivially without asserting anything meaningful | WARNING |
| Orphaned test with no AC mapping | WARNING |
| Stale test referencing changed AC | WARNING |

Run `.agent/workflows/qa-validation-check.md` to systematically check these.

---

## 9. Test-First Preservation

Line 11 does not weaken Line 4 test-first requirements.

The existing sequence is preserved:

```
Approved spec → ACs → spec-derived test cases → reviewed executable tests
→ confirmed RED → implementation → GREEN → QA validation
```

Line 11 adds:
- QA judgement on whether the test suite is sufficient
- UAT traceability
- AI-generated test review
- Integration/regression coverage governance
- Security verification (see `int-sdd-security.md`)

Line 11 does NOT change:
- RED must be confirmed before implementation
- Tests derive from ACs, not from code
- GREEN proves implementation satisfies the test; QA evaluates whether the test was sufficient
- Human accountability remains mandatory

---

## 10. Integration and Regression Governance

### 10.1 When to update `_integration.md`

Update the integration test cases when:
- A new feature introduces cross-feature interactions
- An existing feature's behaviour changes in a way that affects other features
- A regression suite for a previously-released capability is needed
- A system-level workflow spans multiple feature specs

### 10.2 Regression on change

When a spec or implementation changes:
- Review which TC and UT IDs may be affected.
- Check whether cross-feature scenarios in `_integration.md` are still valid.
- Explicitly identify regression scope before implementation restarts.

### 10.3 Regression is not catch-all

Do not add speculative regression tests for "things that might break."
Regression coverage should be justified by actual risk of inter-feature impact.

---

## 11. QA Failure Escalation

When QA identifies a failure, classify it before taking action:

| Classification | Signs | Correct action |
| -------------- | ----- | -------------- |
| **A — Test defect** | Test is wrong (incorrect assertion, wrong setup) | Fix the test within task review |
| **B — Implementation defect** | Code fails a correct test | Fix implementation within task scope |
| **C — Task defect** | Task boundary does not encompass required behaviour | Fix the task; return to task readiness |
| **D — Plan defect** | Technical approach prevents required behaviour | Fix the plan |
| **E — Specification defect** | AC is missing, ambiguous, or untestable | Fix the spec through Gate 1 process |
| **F — Architecture/ADR defect** | Architectural constraint prevents required behaviour | Resolve through architecture-check |
| **H — Context/data issue** | Test fails due to environment or data problem | Fix environment/data; re-run |

**Do not blindly patch implementation.** Trace to the correct level before acting.

Examples:
- Test exposes missing required behaviour → inspect spec (E).
- Test exposes unclear permission model → inspect spec / architecture / ADR (E or F).
- UAT exposes unrepresented required behaviour → investigate spec gap (E).

Preserve Line 10's stop-and-escalate discipline.

---

## 12. Human Accountability in QA

- Human QA review is required before Gate 2.
- AI-generated tests that pass do not constitute QA approval.
- AI generation alone does not substitute for QA judgement.
- Passing tests prove implementation satisfies those tests — not that the tests were sufficient.
- A human QA reviewer or engineer is accountable for Gate 2 quality evidence.

---

## 13. Cross-Line Integration

| Existing governance | Where | Line 11 relationship |
| ------------------- | ----- | -------------------- |
| Test-first, RED evidence | Line 4 / non-negotiables.md | Preserved; RED must precede implementation |
| Generate tests workflow | `generate-tests.md` | Extended by QA rule; AC→test derivation preserved |
| Code review | `code-review.md` | AC verification and test evidence are review requirements |
| Gate 2 | `lifecycle.md` | QA evidence (AC trace, test results, UAT) is Gate 2 input |
| Naming/identity | `int-sdd-naming.md` | TC/UT IDs stable; no renumbering; retired IDs not reused |
| Security | `int-sdd-security.md` | Security verification is a separate Gate 2 dimension |
