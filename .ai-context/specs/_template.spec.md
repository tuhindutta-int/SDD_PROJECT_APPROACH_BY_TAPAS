# Spec: &lt;Feature Name&gt;

&gt; **Template only.** Copy to `.ai-context/specs/<feature-slug>.spec.md` when a
&gt; validated BRD requirement is ready for specification. Replace **all**
&gt; placeholders; do not treat any template text as a stated requirement.
&gt;
&gt; Run `.agent/workflows/spec-prewriting-gate.md` before opening this template.
&gt; Follow `.agent/rules/int-sdd-specification-standard.md`.
&gt; Run `.agent/workflows/spec-review.md` before entering Gate 1.

---

## Spec ID

`<feature-slug>`

_Kebab-case, 3–5 words, human-readable, feature-oriented. Remains the
traceability key across all downstream artefacts. Do not reuse or change once
downstream artefacts exist._

---

## Status

`Draft`

_Transition only through the lifecycle defined in `.ai-context/lifecycle.md`:_

```
Draft → In Peer Review → Changes Requested → In Peer Review → Approved
→ Plan Drafted → Plan Reviewed → Tasks Generated → In Development → In QA
→ Ready for Release → Released (vX.Y.Z) → Deprecated / Superseded
```

---

## Revision

`<semantic revision or repository revision reference>`

_Update when Gate 1 feedback materially changes this specification. Keep revision
history visible; do not silently alter approved intent._

---

## Linked BRD

`.ai-context/BRD.md#BRD-NNN`

_Every spec must link to its originating validated BRD entry. Do not make this
spec the first place the business requirement appears. Do not invent a BRD entry._

---

## Intent

`<One unambiguous paragraph: what changes, for whom, under what condition, and the
expected observable outcome. Must be specific enough that two competent engineers
reading it cannot build materially different behaviour. Do not use vague phrases
such as "improve security", "make it faster", or "enhance the experience".>`

---

## Context

- **Constitution constraints:** `.ai-context/constitution.md#<relevant-section>`
- **Architecture context:** `.ai-context/architecture.md#<relevant-section>`
- **Related specifications:** `<relative paths or None>`
- **Relevant ADRs:** `<relative paths or None>`
- **External API contract reference, if applicable:** `<relative path or None>`

_Link to authoritative artefacts. Do not duplicate large blocks of repository
context here. Verify that referenced specs exist and have usable status._

---

## API Contract

&lt;!-- Required ONLY when this feature exposes or consumes an API.
     If no API surface: replace this entire section with the line below. --&gt;

Not applicable — no API surface is specified for this feature.

&lt;!-- If an API surface exists, use the structure below.
     Product-level API behaviour belongs here.
     Implementation mechanics (how it is built) belong in the plan.
     Do not invent payload fields, response shapes, or status codes.
     If the contract is incomplete, mark items as Requires decision. --&gt;

### `<feature-slug>.API01` — `<METHOD> <path>`

**Request payload:**

```json
{
  "field": "type"
}
```

**Success response (`<status code>`):**

```json
{
  "field": "type"
}
```

**Exceptions and validation behaviour:**

| Status    | Condition               | Response body shape       |
| --------- | ----------------------- | ------------------------- |
| `<4xx>`   | `<specified condition>` | `<specified shape>`       |
| `<5xx>`   | `<specified condition>` | `<specified shape>`       |

---

## Acceptance Criteria

_Each criterion must be objectively observable and testable. Use stable IDs.
Use Given/When/Then where applicable. Include applicable failure paths or
explicitly flag them as unresolved. Do not use adjectives ("secure", "fast",
"robust", "good") as substitutes for observable behaviour._

1. `<feature-slug>.AC1` — Given `<precondition>`, when `<action>`, then
   `<observable outcome>`.

2. `<feature-slug>.AC2` — Given `<precondition>`, when `<action>`, then
   `<observable outcome>`.

**Failure paths to address where applicable:**

- Invalid input
- Missing required input
- Unauthorized access
- Authentication failure
- Validation failure
- Duplicate request
- Boundary values
- Error / exception states
- Partial failure
- Relevant state transitions
- Timeout / failure behaviour
- Important null / undefined conditions

_If a required failure path is not yet defined: flag it as
`Requires decision` — do not invent expected behaviour._

---

## Spec-Derived Unit Test Cases

_Derived from acceptance criteria above. Direction: Spec → AC → Test.
Broader QA (negative testing, data-volume, browser/device matrices,
cross-feature regression) belongs in
`.ai-context/test_cases/<feature-slug>.test_cases.md`._

| Test ID                  | Maps to AC               | Scenario       | Expected result  |
| ------------------------ | ------------------------ | -------------- | ---------------- |
| `<feature-slug>.UT01`    | `<feature-slug>.AC1`     | `<scenario>`   | `<expected>`     |
| `<feature-slug>.UT02`    | `<feature-slug>.AC2`     | `<scenario>`   | `<expected>`     |

---

## Explicitly Out of Scope

_This section is mandatory. Explicit exclusions prevent scope creep. State
adjacent functionality that is NOT part of this feature where ambiguity is
likely. Do not leave major scope exclusions implicit._

- `<behaviour explicitly excluded from this feature>`

---

## Non-Functional Constraints

_Include only constraints that are sourced from the constitution, BRD, or an
approved decision. Do not invent targets or use vague aspirations. If a
relevant constraint is unresolved, mark it `Requires decision`._

- `<constraint — source: .ai-context/constitution.md#<section> or Requires decision>`

---

## Open Items and Decisions Required

_List every question that is not resolved at spec-authoring time. Use:
`Not specified`, `Conflicting`, `Requires decision`, or `TBD`.
Do not convert any open item into a stated fact in the spec above._

- `<item — Not specified | Conflicting | Requires decision | TBD>`

---

## Gate Approvals and History

_Gate 1 review is conducted using `.agent/workflows/gate-1-review.md`.
The automated pre-submission check (`spec-review.md` → READY FOR GATE 1) is a
pre-condition, not Gate 1 approval. Three distinct events apply:_

_1. `spec-review.md` → READY FOR GATE 1 (automated, author-run)_
_2. `gate-1-review.md` → reviewer analysis complete (human reviewer)_
_3. Named human non-author records APPROVED in this table (Gate 1 approval)_

| Gate   | Named human reviewer    | Reviewer ≠ author | Outcome                          | Gate 1 review record      | Date        |
| ------ | ----------------------- | ------------------ | -------------------------------- | ------------------------- | ----------- |
| Gate 1 | `<name>`                | `<yes / no>`       | `<Approved / Changes Requested>` | `<path or inline record>` | `<YYYY-MM-DD>` |
| Gate 2 | `<name or Not started>` | `<yes / no>`       | `<outcome>`                      | `<path or inline record>` | `<YYYY-MM-DD>` |

_Only a named human non-author can produce Gate 1 `Approved` status.
Codex/Claude running `gate-1-review.md` is reviewer assistance, not approval._

---


## Boundary

This spec defines **what** is built and what correct means. It must not contain:

- Implementation approach, framework choices, class names, database structures,
  internal algorithms, source layout, library selection, call sequences.
- Task lists, coding prompts, or architecture dumps.
- Speculative implementation decisions.

Only preserve a technical detail here if it is already an approved contractual
product or project constraint.

The next artefact is `.ai-context/plans/<feature-slug>.plan.md`, created only
after recorded Gate 1 approval.
