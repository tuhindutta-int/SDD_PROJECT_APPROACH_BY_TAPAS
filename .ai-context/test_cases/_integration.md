# Integration and System-Level Test Cases

> This file documents cross-feature journeys, system-level scenarios, and
> regression suites that span multiple feature specifications. It is owned by
> QA governance under INT SDD Line 11.
>
> Governing rule: `.agent/rules/int-sdd-qa.md` §4.2
> QA validation: `.agent/workflows/qa-validation-check.md` Part 6

---

## Purpose and Scope

This file is the correct location for:

- Cross-feature user journeys and workflows
- System-level scenarios spanning multiple specs
- Regression suites for released capabilities
- Scenarios that cannot be cleanly owned by one feature spec
- Cross-feature negative and boundary paths

This file is NOT:
- A duplicate of feature-level unit tests (those belong in `<slug>.test_cases.md`)
- A catch-all for untraceable scenarios
- A replacement for per-feature AC traceability

Each scenario must identify which features/specs it spans.

---

## Integration Scenario Structure

### `INT-NN` — `<short human-readable scenario name>`

| Field | Value |
| ----- | ----- |
| **Scenario ID** | `INT-NN` |
| **Spans features** | `<slug-a>`, `<slug-b>` (list all relevant feature slugs) |
| **Traces to ACs** | `<slug-a>.ACN`, `<slug-b>.ACM` (list all relevant AC IDs) |
| **Scenario type** | `Cross-feature journey / System-level / Regression` |
| **Preconditions** | `<system state and data required>` |
| **Steps** | `<numbered, reproducible steps>` |
| **Expected result** | `<explicit observable outcome>` |
| **Pass/fail criterion** | `<how to determine pass or fail>` |
| **Notes** | `<risk observations, QA judgement, grey-area flags>` |

---

## Active Integration Scenarios

_No integration scenarios exist yet. This file is initialized at governance
bootstrap. Add scenarios here when features with cross-feature interactions
are specified and approved._

---

## Identity Rules

- Scenario IDs use the format `INT-NN` (e.g., `INT-01`, `INT-02`).
- IDs are stable — do not renumber when scenarios are reordered.
- Retired scenario IDs are not reassigned.
- Each scenario must explicitly name the feature slugs it spans.
- Each scenario must trace to at least one AC ID where possible.
- Scenarios without AC traces must document why they cannot be traced
  (e.g., system-level behaviour emerging from multiple features, no single
  owning AC).

---

## Regression Baseline

_No regression baseline is established yet. When features reach `Released`
status, their relevant integration scenarios form the regression baseline.
Update this section when the first feature is released._

