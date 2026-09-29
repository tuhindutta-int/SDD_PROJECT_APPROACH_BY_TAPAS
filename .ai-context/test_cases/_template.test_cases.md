# Feature Test Cases Template

> Template only. Copy to `.ai-context/test_cases/<feature-slug>.test_cases.md`
> when test cases are being created for an approved feature specification.
> Replace all placeholders. Do not treat template text as approved test content.
>
> The spec slug must match the slug in `.ai-context/specs/<feature-slug>.spec.md`
> and all other feature artefacts. Confirm slug consistency using
> `.agent/workflows/naming-identifier-check.md` before proceeding.

---

## Feature Slug

`<feature-slug>`

_This must match exactly the Spec ID in the corresponding spec file and the
filenames of all other feature artefacts._

## Derived From

- **Approved spec:** `.ai-context/specs/<feature-slug>.spec.md`
- **Approved plan:** `.ai-context/plans/<feature-slug>.plan.md`

---

## Test Case Coverage Overview

| AC ID | Coverage status | Notes |
| ----- | --------------- | ----- |
| `<feature-slug>.AC1` | `Covered / Partial / Not yet covered` | |

---

## Test Cases

### `<feature-slug>.TC01` — `<short human-readable test case name>`

| Field | Value |
| ----- | ----- |
| **Test case ID** | `<feature-slug>.TC01` |
| **Type** | `Unit / Integration / E2E / Manual` |
| **Maps to AC** | `<feature-slug>.AC1` |
| **Maps to UT** | `<feature-slug>.UT01` |
| **Preconditions** | `<explicit preconditions>` |
| **Test steps** | `<numbered steps>` |
| **Expected result** | `<explicit observable outcome>` |
| **Pass/fail criterion** | `<how to determine pass or fail>` |
| **RED confirmed** | `<Yes / No / Not yet run>` |
| **GREEN confirmed** | `<Yes / No / Not yet run>` |

---

### `<feature-slug>.TC02` — `<short human-readable test case name>`

| Field | Value |
| ----- | ----- |
| **Test case ID** | `<feature-slug>.TC02` |
| **Type** | `Unit / Integration / E2E / Manual` |
| **Maps to AC** | `<feature-slug>.AC2` |
| **Maps to UT** | `<feature-slug>.UT02` |
| **Preconditions** | |
| **Test steps** | |
| **Expected result** | |
| **Pass/fail criterion** | |
| **RED confirmed** | `No` |
| **GREEN confirmed** | `No` |

---

## Failure Path Coverage

| Failure path | AC ID | Test case ID | Covered? |
| ------------ | ----- | ------------ | -------- |
| Invalid input | `<feature-slug>.ACN` | `<feature-slug>.TCNN` | `Yes / No` |
| Unauthorized access | | | |
| Boundary values | | | |
| Timeout / service failure | | | |

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

## Boundary

This artefact documents broader QA test cases derived from acceptance criteria.
Spec-level unit test mappings (`<slug>.UT01`) are defined in the spec.
This file does NOT redefine acceptance criteria or add product requirements.
Downstream artefact for implementation: `.ai-context/tasks/<feature-slug>.tasks.md`.
