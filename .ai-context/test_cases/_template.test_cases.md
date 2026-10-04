# Feature Test Cases Template

> Template only. Copy to `.ai-context/test_cases/<feature-slug>.test_cases.md`
> when test cases are being created for an approved feature specification.
> Replace all placeholders. Do not treat template text as approved test content.
>
> The spec slug must match the slug in `.ai-context/specs/<feature-slug>.spec.md`
> and all other feature artefacts. Confirm slug consistency using
> `.agent/workflows/naming-identifier-check.md` before proceeding.
>
> Governing rules: `.agent/rules/int-sdd-qa.md`
> Generation workflow: `.agent/workflows/generate-tests.md`
> QA validation: `.agent/workflows/qa-validation-check.md`
> Security (test fixtures): `.agent/workflows/security-check.md` Part 1

---

## Feature Slug

`<feature-slug>`

_This must match exactly the Spec ID in the corresponding spec file and the
filenames of all other feature artefacts._

## Derived From

- **Approved spec:** `.ai-context/specs/<feature-slug>.spec.md`
- **Approved plan:** `.ai-context/plans/<feature-slug>.plan.md`

**IMPORTANT: Tests must be derived from the approved spec, not from reading
the finished implementation. See `.agent/rules/int-sdd-qa.md` §2.**

---

## AC Coverage Matrix

| AC ID | Coverage status | Test IDs | Notes |
| ----- | --------------- | -------- | ----- |
| `<feature-slug>.AC1` | `Covered / Partial / Not yet covered` | `<slug>.TC01, .UT01` | |
| `<feature-slug>.AC2` | `Not yet covered` | | |

---

## Test Identity Reference

| Namespace | Format | Location | Purpose |
| --------- | ------ | -------- | ------- |
| **TC** | `<slug>.TCNN` | This file | QA test-case identity — broader validation scenarios including negative, boundary, manual, UAT |
| **UT** | `<slug>.UTNN` | Spec file | Spec-level unit-test identity — maps to executable test functions |

IDs are stable. Do not renumber. Retired IDs are never reassigned.

---

## Test Cases

### `<feature-slug>.TC01` — `<short human-readable test case name>`

| Field | Value |
| ----- | ----- |
| **Test case ID** | `<feature-slug>.TC01` |
| **Type** | `Unit / Integration / E2E / Manual / UAT` |
| **Maps to AC** | `<feature-slug>.AC1` |
| **Maps to UT** | `<feature-slug>.UT01` |
| **Preconditions** | `<explicit system state and data before test>` |
| **Test steps** | `<numbered, reproducible steps>` |
| **Expected result** | `<explicit observable outcome — from spec, not code>` |
| **Pass/fail criterion** | `<how to objectively determine pass or fail>` |
| **RED confirmed** | `Yes / No / Not yet run` |
| **GREEN confirmed** | `Yes / No / Not yet run` |
| **Negative/boundary notes** | `<relevant failure paths, edge cases, or boundary conditions>` |
| **Security notes** | `<N/A or security-relevant check required by this test>` |
| **QA notes** | `<grey-area flags, risk observations, QA judgement record>` |

---

### `<feature-slug>.TC02` — `<short human-readable test case name>`

| Field | Value |
| ----- | ----- |
| **Test case ID** | `<feature-slug>.TC02` |
| **Type** | `Unit / Integration / E2E / Manual / UAT` |
| **Maps to AC** | `<feature-slug>.AC2` |
| **Maps to UT** | `<feature-slug>.UT02` |
| **Preconditions** | |
| **Test steps** | |
| **Expected result** | |
| **Pass/fail criterion** | |
| **RED confirmed** | `No` |
| **GREEN confirmed** | `No` |
| **Negative/boundary notes** | |
| **Security notes** | `N/A` |
| **QA notes** | |

---

## Failure Path Coverage

| Failure path | AC ID | Test case ID | Covered? |
| ------------ | ----- | ------------ | -------- |
| Invalid input | `<feature-slug>.ACN` | `<feature-slug>.TCNN` | `Yes / No` |
| Unauthorized access | | | |
| Boundary values | | | |
| Timeout / service failure | | | |
| Security-relevant failure (injection, auth, data) | | | |

---

## Broader QA Scenarios

> Document QA scenarios that extend beyond individual ACs (data volume,
> compatibility, accessibility, exploratory, regression). These must not be
> presented as new acceptance criteria.

| QA scenario | Type | Rationale | Outcome |
| ----------- | ---- | --------- | ------- |
| `<scenario description>` | `Data volume / Browser / Accessibility / Exploratory / Regression` | `<why this extends beyond a single AC>` | `<pass / fail / pending>` |

If no broader QA scenarios apply: state `None — scope does not require broader QA coverage beyond AC-derived tests.`

---

## UAT Traceability

> For each UAT scenario, record the mapping to AC IDs.
> A UAT scenario with no AC trace is a potential specification gap.

| UAT reference | Traces to AC IDs | Classification |
| ------------- | ---------------- | -------------- |
| `<UAT scenario ref>` | `<slug>.ACN` | `Traced / Specification gap` |

---

## Test Identity Rules

Test case IDs follow the pattern `<feature-slug>.TCNN` where:
- The slug prefix matches the feature slug exactly.
- `TC` is the type code for test cases in this artefact.
- `NN` is a zero-padded two-digit sequential number.
- IDs are stable identities — do not renumber when items are reordered.
- Retired test IDs must not be reassigned.

The unit test IDs (`<feature-slug>.UT01` etc.) are defined in the spec.
The test case IDs (`<feature-slug>.TC01` etc.) are defined here and map to the spec's UT IDs.

---

## AI-Generated Test Review Record

If any tests were AI-generated, record the QA review:

- [ ] All AI-generated tests reviewed for AC mapping accuracy
- [ ] All AI-generated tests reviewed for meaningful assertions
- [ ] Negative-path coverage augmented by QA where required
- [ ] Boundary-condition coverage augmented by QA where required
- [ ] Passing AI-generated tests not treated as QA closure

QA reviewer: `<name or role>`
Review date: `YYYY-MM-DD`

---

## Boundary

This artefact documents QA test cases derived from acceptance criteria.
Spec-level unit test mappings (`<slug>.UT01`) are defined in the spec.
This file does NOT redefine acceptance criteria or add product requirements.

Cross-feature and system-level scenarios belong in:
`.ai-context/test_cases/_integration.md`

Security verification belongs in:
`.agent/workflows/security-check.md`
