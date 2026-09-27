# INT SDD Specification Standard

## Authority

This is the authoritative repository rule for feature-specification quality and
governance. One feature equals one specification. A spec defines feature intent
and correctness; it is not a plan, coding prompt, task list, architecture diary,
or repository-context dump.

Supplement: `.ai-context/constitution.md` (project-wide law).
Template: `.ai-context/specs/_template.spec.md`.
Pre-writing gate: `.agent/workflows/spec-prewriting-gate.md`.
Review workflow: `.agent/workflows/spec-review.md`.

---

## 1. Feature Boundary — One Feature, One Spec

- One spec represents one bounded, independently reviewable and deliverable
  feature.
- If a requested change contains multiple independent features, **stop and
  identify the required decomposition** rather than silently bundling them.
- Examples of too-broad scope (not acceptable): "The user module", "Checkout
  system", "Platform improvements", "Improve security".
- Examples of bounded scope (acceptable): "OTP-based 2FA for existing users",
  "Password reset via email link", "Rate limiting on the login endpoint".
- If a request appears to be one feature but can reasonably be decomposed into
  features that are independently deliverable, identify the decomposition.

---

## 2. Feature Identity — Slug Standard

A spec ID is a unique, human-readable kebab-case `<feature-slug>`.

Rules:

- Kebab-case.
- Human-readable, 3–5 words.
- Specific enough to remain unique across the project lifetime.
- Feature-oriented, not a vague generic identifier.
- Never use: `feature-42`, `misc-change`, `new-feature`, `fix-stuff`.
- Do not silently reuse or change a slug once downstream artefacts exist.

The slug is the traceability key across all artefacts:

```
.ai-context/specs/<feature-slug>.spec.md
.ai-context/plans/<feature-slug>.plan.md
.ai-context/tasks/<feature-slug>.tasks.md
.ai-context/test_cases/<feature-slug>.test_cases.md
feature/<feature-slug>
<feature-slug>.AC1, .AC2, .ACN
<feature-slug>.API01, .API02, .APINN
<feature-slug>.UT01, .UT02, .UTNN
<feature-slug>.T01, .T02, .TNN
```

---

## 3. Pre-Writing Gate — Mandatory

Before drafting or revising a spec, run `.agent/workflows/spec-prewriting-gate.md`.

The nine pre-writing questions must be explicitly answered:

1. Can the feature be described in one unambiguous paragraph?
2. Is the business requirement validated in `.ai-context/BRD.md`?
3. Are all required context artefacts available?
4. Are related specifications identified?
5. Are architecture constraints known?
6. Are design and system constraints known?
7. Is the API contract available (where applicable)?
8. Are required stakeholders and sign-offs known?
9. Are unresolved decisions explicitly identified?

If a critical answer is unavailable:

**STOP. Do not finalize the spec. Report the missing information.**

The business requirement must first be captured in `.ai-context/BRD.md`; the
correct next action for a missing requirement is Discovery / BRD, not
spec authoring.

---

## 4. BRD → Spec Relationship

A feature specification must derive from a validated BRD entry.

The required relationship:

```
BRD-NNN
   ↓
<feature-slug>
   ↓
.ai-context/specs/<feature-slug>.spec.md
```

- Do not make the spec the first place a business requirement appears.
- Do not invent BRD entries.
- Do not create a spec for a requirement with no BRD entry.

---

## 5. Spec Purpose — The Feature Contract

The spec must answer:

1. What are we building?
2. For whom?
3. Under what conditions?
4. What does correct behaviour mean?
5. What is explicitly out of scope?
6. What API behaviour is contractual (where an API surface exists)?
7. What testable conditions prove correctness?

**SPEC = WHAT and CORRECTNESS.**
**PLAN = HOW.**
**TASKS = EXECUTION SEQUENCE.**

Do not allow the spec to become an implementation plan, coding prompt, task list,
technical design document, or architecture dump.

---

## 6. Required Content Order

Use the template at `.ai-context/specs/_template.spec.md`.

Required sections in order:

1. `# Spec: <Feature Name>`
2. `## Spec ID` — the `<feature-slug>`
3. `## Status` — explicit lifecycle state
4. `## Revision` — version marker
5. `## Linked BRD` — `BRD-NNN` reference
6. `## Intent` — one paragraph
7. `## Context` — references, not duplicated content
8. `## API Contract` — conditional (required when API surface exists)
9. `## Acceptance Criteria` — testable, with stable IDs and failure paths
10. `## Spec-Derived Unit Test Cases` — mapped to AC IDs
11. `## Explicitly Out of Scope` — mandatory
12. `## Non-Functional Constraints` — sourced from constitution / decisions
13. `## Open Items and Decisions Required` — explicit, not hidden as facts
14. `## Gate Approvals and History` — required for Gate 1 tracking

The order is intentional: **intent and acceptance criteria come before
implementation-oriented technical detail.**

---

## 7. Intent Standard

Intent must be:

- One concise paragraph.
- Unambiguous and feature-specific.
- Behavior-focused: states what changes, for whom, under what condition, and
  expected outcome.

Not acceptable as intent:

- "Improve security."
- "Make the application faster."
- "Improve the login experience."
- "Make the dashboard robust."

These are goals or adjectives — not testable feature behaviour.

**When intent can be interpreted in multiple materially different ways: STOP.
Mark the spec as not ready. Return to BRD and Discovery.**

---

## 8. Acceptance Criteria Standard

Every acceptance criterion must:

- Have a stable `<feature-slug>.ACN` ID.
- Describe observable behaviour.
- Be objectively verifiable.
- Be specific enough for test generation.
- Avoid adjectives as substitutes for behaviour.

Preferred structure:

```
<feature-slug>.AC1 —
Given <precondition>,
when <action>,
then <observable outcome>.
```

Not acceptable:

- "System should be secure."
- "API should be fast."
- "Component should be robust."
- "Login should work properly."
- "Performance should be good."

These must be converted into observable, testable behaviour or explicitly
identified as unresolved requirements.

### Failure Paths

The spec must not only describe the happy path. Address where applicable:

- Invalid input
- Missing required input
- Unauthorized access
- Authentication failure
- Validation failure
- Duplicate requests
- Boundary values
- Error states
- Partial failure
- Relevant state transitions
- Timeout / failure behaviour
- Important null / undefined conditions

If required failure-path behaviour is unresolved: **flag it explicitly**.
Do not invent expected behaviour.

---

## 9. API Contract Rule

**MANDATORY when the feature exposes or consumes an API.**

When the feature has an API surface, the spec must define each contract using
stable `<feature-slug>.APINN` identifiers. Each entry must include:

- Method and path (or external operation name).
- Request payload shape.
- Success response shape and status code.
- Exception conditions, error codes, and error response shapes.
- Relevant validation behaviour.

The contract must be sufficient to test associated ACs.

**When no API surface exists:** state explicitly:
`Not applicable — no API surface is specified.`
Do not invent an API merely to complete the template section.

**SPEC defines the API contract. PLAN decides the implementation approach.**

Do not allow the implementation plan to silently invent payload fields, response
shapes, status codes, or error semantics that are not defined here.

---

## 10. Explicit Out of Scope — Mandatory

Every specification must have an `## Explicitly Out of Scope` section.

This section prevents scope creep. Explicitly state adjacent functionality that
is NOT part of the feature where ambiguity is likely.

Do not leave major scope exclusions implicit.

---

## 11. Spec-Derived Test Cases

The specification must contain a unit test case table derived directly from the
acceptance criteria.

Use stable test IDs: `<feature-slug>.UTNN`.

| Test ID                | Maps to AC           | Scenario       | Expected result  |
| ---------------------- | -------------------- | -------------- | ---------------- |
| `<feature-slug>.UT01`  | `<feature-slug>.AC1` | `<scenario>`   | `<expected>`     |

Direction must be: **Spec → AC → Test**. Not: Code → Test → pretend requirement.

Broader QA concerns (negative testing, data-volume, browser/device matrices,
accessibility, cross-feature regression, system-level journeys) belong in
`.ai-context/test_cases/<feature-slug>.test_cases.md`. Maintain traceability
back to the spec.

---

## 12. Non-Functional Constraints

Where the constitution or project context establishes non-functional requirements,
reference them in the spec.

Do not invent numbers. Do not create arbitrary targets.

Use the authoritative source reference (e.g., "from `.ai-context/constitution.md`").

If the requirement is unresolved: mark it `Requires decision`.

---

## 13. Spec Status and Versioning

Specifications are first-class version-controlled artefacts.

Valid status values (from `.ai-context/lifecycle.md`):

```
Draft
In Peer Review
Changes Requested
Approved
Plan Drafted
Plan Reviewed
Tasks Generated
In Development
In QA
Ready for Release
Released (vX.Y.Z)
Deprecated / Superseded
```

Do not use: "Almost Ready", "Reviewed", "Pending", "Looks Good".

When Gate 1 feedback materially changes a spec: update the revision marker,
keep the change visible, do not silently alter approved intent.

---

## 14. Spec Change Propagation

A material specification change can invalidate downstream artefacts.

**Material change** = a change that alters intended behaviour, acceptance
criteria, API contract, scope, or out-of-scope boundaries.

When a spec changes materially after planning or task generation has begun:

1. Identify potentially stale downstream artefacts:
   - Plan (`.ai-context/plans/<feature-slug>.plan.md`)
   - Tasks (`.ai-context/tasks/<feature-slug>.tasks.md`)
   - Test cases (`.ai-context/test_cases/<feature-slug>.test_cases.md`)
   - Executable tests
   - Any in-progress or completed implementation

2. **Do not automatically rewrite all downstream artefacts.**

3. Report which artefacts are affected and require human decision before
   updating them.

Stale plan, stale tasks, stale tests, and stale implementation assumptions must
not continue unnoticed.

---

## 15. Gate 1 Readiness

Run `.agent/workflows/spec-review.md` before entering Gate 1.

A spec may enter peer review only when all required checks pass.

Gate 1 readiness does not mean Gate 1 approval. Gate 1 approval is a named
human, non-author governance event recorded in the spec's Gate Approvals table.

**Codex/Claude cannot self-approve a specification.**

---

## 16. Anti-Patterns to Block

The review workflow (`.agent/workflows/spec-review.md`) must flag and block:

- Multiple independent features bundled into one spec
- Giant system-wide or module-wide specifications
- Vague or adjective-only intent
- Acceptance criteria without stable IDs
- "Secure", "fast", "robust", "good" used as acceptance criteria
- Requirements hidden inside technical implementation prose
- Implementation instructions disguised as requirements
- Missing API contract when an API surface exists
- API payload omitted
- API success response omitted
- API exception conditions omitted
- Happy-path-only acceptance criteria with no failure paths
- Missing Explicitly Out of Scope section
- Invented requirements with no BRD source
- Unresolved decisions written as stated facts
- Spec copied from code; code treated as the authoritative definition
- Large blocks of unrelated repository context pasted into the spec
- Implementation details forced into the spec as feature intent
- Missing stable AC IDs
- Missing BRD linkage
- Dependencies not identified or unresolvable
- Fake, meaningless, or placeholder-only spec content

---

## 17. Context References

Do not dump repository context into every specification.

Prefer references:

- `.ai-context/BRD.md`
- `.ai-context/architecture.md`
- `.ai-context/constitution.md`
- `.ai-context/specs/<related>.spec.md`
- `.ai-context/decisions/ADR-NNN.md`

Link to the authoritative artefact rather than duplicating large documents.

---

## 18. No Implementation Detail Leak

The specification must not become the plan.

Do not put the following into the spec unless they are already an approved,
contractual architectural or product constraint:

- "Use Redis"
- "Create class X"
- "Use React Context"
- "Call service Y directly"
- "Store the value in table Z"
- "Use package X"
- "Publish to Kafka topic Y"

**SPEC = WHAT. PLAN = HOW.**
