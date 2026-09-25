# Feature Specification Template

> Template only. Copy to `.ai-context/specs/<feature-slug>.spec.md` when a validated BRD requirement is ready for specification. Replace all placeholders; do not treat template text as a requirement. Follow `.agent/rules/int-sdd-specification-standard.md` and run `.agent/workflows/spec-review.md` before Gate 1.

## Spec ID

`<feature-slug>`

## Status and Revision

- **Status:** `Draft` — transition only through the lifecycle defined in `.ai-context/lifecycle.md`.
- **Revision:** `<semantic revision or repository revision reference>`

## Linked BRD

`.ai-context/BRD.md#BRD-NNN`

## Intent

`<One unambiguous paragraph: what changes, for whom, under what condition, and expected observable outcome.>`

## Context / References

- Constitution constraints: `.ai-context/constitution.md#<relevant-section>`
- Architecture context: `.ai-context/architecture.md#<relevant-section>`
- Related specifications: `<relative paths or None>`
- Relevant ADRs: `<relative paths or None>`
- External API contract reference, if applicable: `<relative path or None>`

## API Contract

Required only when this feature exposes or consumes an API. Otherwise state: `Not applicable — no API surface is specified.`

### `<feature-slug>.API01` — `<method and path or external operation>`

**Request payload:**

```json
{ "field": "type" }
```

**Success response (`<status code>`):**

```json
{ "field": "type" }
```

**Exceptions and validation behaviour:**

| Status | Condition | Response body |
| --- | --- | --- |
| `<4xx/5xx>` | `<specified condition>` | `<specified shape>` |

## Acceptance Criteria

Each criterion must be objectively observable and testable. Use stable identifiers and Given/When/Then where applicable. Include applicable failure paths or list the unresolved decision.

1. `<feature-slug>.AC1` — Given `<precondition>`, when `<action>`, then `<observable outcome>`.
2. `<feature-slug>.AC2` — `<criterion or remove until validated>`.

## Spec-Derived Unit Test Cases

| Test ID | Acceptance criterion | Scenario | Expected result |
| --- | --- | --- | --- |
| `<feature-slug>.UT01` | `<feature-slug>.AC1` | `<scenario>` | `<expected result>` |

## Explicitly Out of Scope

- `<behaviour explicitly excluded from this feature>`

## Non-Functional Constraints

- `<constraint traceable to constitution, BRD, or approved decision>`

## Open Items and Decisions Required

- `<Not specified | Conflicting | Requires decision | TBD>`

## Gate Approvals & History

| Gate | Named human reviewer | Reviewer differs from author | Outcome | Evidence / date |
| --- | --- | --- | --- | --- |
| Gate 1 | `<name>` | `<yes/no>` | `<Approved / Changes Requested>` | `<repository record>` |
| Gate 2 | `<name or Not started>` | `<yes/no>` | `<outcome>` | `<repository record>` |

## Boundary

This spec defines **what** is built and what correct means. It does not prescribe the implementation approach unless a technical constraint is an approved product or project decision. The next artefact is `.ai-context/plans/<feature-slug>.plan.md`, created only after recorded Gate 1 approval.
