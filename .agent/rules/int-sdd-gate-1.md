# INT SDD Gate 1 — Spec and Plan Peer Review

## Authority

This is the authoritative repository rule for Gate 1 governance.
Gate 1 is the mandatory human peer review of a feature specification — and,
where present, its plan — before any implementation work begins.

Related artefacts:
- Workflow: `.agent/workflows/gate-1-review.md` — run during Gate 1.
- Spec standard: `.agent/rules/int-sdd-specification-standard.md`.
- Lifecycle: `.ai-context/lifecycle.md` — defines state transitions.
- Constitution: `.ai-context/constitution.md` — the project-wide law.

---

## 1. What Gate 1 Is

Gate 1 is **PRE-CODE DESIGN AND INTENT VALIDATION** — not post-code bug finding.

The review occurs BEFORE implementation begins. Its purpose is to catch defects
in the specification (and plan, where present) before they propagate into tests,
implementation, documentation, and production.

The defect propagation chain Gate 1 exists to break:

```
Spec error
    ↓ Plan error
        ↓ Test error
            ↓ Code error
                ↓ Production error
```

A spec defect that reaches code is exponentially more expensive to fix than a
spec defect found at Gate 1.

---

## 2. What Gate 1 Is NOT

Gate 1 is NOT:

- A check that the spec file exists.
- A check that someone read the spec.
- Approval by the spec author.
- An automated readiness report from an AI tool.
- Gate 2 (which reviews the actual implementation diff).

`READY FOR GATE 1` from the automated `spec-review.md` workflow is a
**pre-submission signal** — not Gate 1 approval.

Gate 1 approval is a **named human non-author review event** recorded in the
spec's Gate Approvals table.

---

## 3. Reviewer Requirements

### 3.1 Named, independent reviewer

- The reviewer must be **explicitly named** and different from the spec author.
- The spec author **cannot** review and approve their own specification.
- AI (Codex/Claude/any agent) **cannot** serve as the human Gate 1 approval authority.
- An AI may assist with automated analysis (see `gate-1-review.md`), but the
  human reviewer remains solely responsible for the Gate 1 decision.

### 3.2 API surface reviewer consideration

When the feature includes an API Contract section:

- At least one Gate 1 reviewer should be someone who will also be a Gate 2
  reviewer for the same feature, OR an appropriate domain SME who understands
  the existing system's API conventions.
- The reason: the reviewer must evaluate not only readability but whether the
  API contract makes sense against existing system behaviour and conventions.

This additional reviewer consideration applies only when an API surface exists.
Do not impose it universally on features with no API surface.

### 3.3 Security / Architecture reviewer

When the feature materially affects security or introduces architectural changes:

- The applicable security reviewer or architecture authority must be named
  in addition to the standard peer reviewer.
- This follows the constitution's requirement that Gate 1 includes security
  and architecture sign-off for affected features.

### 3.4 Identity record

Every Gate 1 review must record:

| Field              | Required value                                     |
| ------------------ | -------------------------------------------------- |
| Spec author        | Named individual                                   |
| Gate 1 reviewer    | Named individual, different from author            |
| Review date        | Explicit date                                      |
| Outcome            | `Approved` or `Changes Requested`                  |
| Evidence reference | Location of Gate 1 record in the spec or repository |

These fields must not collapse into one identity. An identity mismatch
(reviewer = author) is an automatic Gate 1 violation.

---

## 4. Gate 1 Review Dimensions

The Gate 1 review evaluates seven dimensions. Every dimension must be
explicitly addressed. Silence on a dimension is not a pass.

### A. Ambiguity

Primary question:
> Could two competent engineers build materially different things from this
> specification?

The reviewer must examine:

- Intent paragraph
- Business rules and conditions
- State transitions
- Edge cases and failure behaviour
- Validation behaviour
- Ownership and data flow
- Dependencies and prerequisite behaviour
- API semantics (when applicable)
- Expected outcomes for each AC

If materially different interpretations of any section are possible: **BLOCK.**

Return the ambiguous section, the competing interpretations, the implementation
impact, and the required clarification. Do not resolve the ambiguity by guessing.

A technically readable spec is not necessarily an unambiguous spec.

Example Gate 1 ambiguity challenge:
> AC: "After 5 failed attempts, invalidate the OTP session."
> Gate 1 must ask: Does the next login attempt start a new counter?
> Is the counter session-based or user-based? What happens after timeout?
> What happens after successful authentication? What happens when the same
> user starts a new session?
> If these answers change implementation behaviour and are not defined: BLOCK.

### B. Testability

Every acceptance criterion must be objectively testable.

The reviewer must verify:

- Stable AC ID exists.
- Given/When/Then structure is used where applicable.
- Observable outcome is defined.
- Expected state is explicit.
- Failure behaviour is explicitly addressed.
- No vague adjectives substitute for observable behaviour.

Ask: *"How would QA prove this AC is satisfied? Can the answer be confirmed by
running a specific test against a specific observable?"*

Vague ACs such as "secure", "fast", "robust", "good", "reliable",
"user-friendly", "optimized" are **BLOCKING** unless backed by an explicit,
measurable, project-defined requirement (e.g., from the constitution or BRD).

### C. Scope

The reviewer must verify:

- One feature and one spec only — not bundled independent capabilities.
- Feature boundary is clearly drawn.
- `Explicitly Out of Scope` section exists and names relevant exclusions.
- Adjacent features are not silently included.
- Deferred work remains deferred.

Look for hidden scope expansion:
- Unrelated refactors hidden in the feature
- Additional API endpoints beyond the stated feature
- Unrelated authentication changes
- Migration work
- Broad performance initiatives
- Unrelated database changes

A feature may reference dependencies, but referencing a dependency is not the
same as owning its scope.

If the feature contains multiple independently deliverable capabilities: **BLOCK.**
Request decomposition before approval.

### D. API Contract (when applicable)

When an API surface exists, the reviewer must verify:

- Method and path are defined.
- Request payload shape is defined.
- Success response shape and status code are defined.
- Exception/error conditions and response shapes are defined.
- Validation behaviour relevant to the contract is defined.
- The contract is internally consistent.
- The contract matches existing project/service API conventions (where those
  conventions are available in `architecture.md` or approved ADRs).
- The contract defines more than just the happy path.

A spec that states "Call POST /something" without defining what is sent and
what comes back: **BLOCK.**

A contract that defines a request but not expected error behaviour where error
behaviour matters: **BLOCK.**

Do not allow the implementation plan to invent missing product-level API
semantics. If the contract is incomplete, the spec is not approved.

When no API surface exists: confirm the spec explicitly states
`Not applicable — no API surface is specified.`

### E. Constitution Compliance

The reviewer must verify the spec and plan against `.ai-context/constitution.md`.

Check:

- Testing discipline constraints (test-first, AC derivation).
- Security requirements (security-relevant behaviour must be explicit and traceable).
- Architectural constraints (no unspecified technology or topology).
- Non-functional baselines (performance, availability, compliance where defined).
- Versioning rules.
- Repository / engineering rules (no secrets/PII, no AI attribution, etc.).

**Silence is not compliance.** If the constitution imposes a meaningful constraint
and the spec or plan is silent about how that constraint is handled: **flag it.**

A plan that violates a constitution rule is a Gate 1 rejection.
Do not defer constitution violations to Gate 2.

### F. Overlap Check

Search existing specs in `.ai-context/specs/` for features that may already
cover some or all of the proposed intent.

Determine:

- Is this feature already specified (duplicate spec)?
- Does another spec contain overlapping acceptance criteria?
- Is the requested change actually a revision of an existing approved spec?
- Would two specs now claim ownership of the same behaviour?

If overlap exists: **do not approve a new spec.** Instead:

- Identify the existing spec.
- Identify the overlapping scope.
- Identify whether this is duplication, conflict, or intended revision.
- Recommend the appropriate governance action (revise existing, merge, etc.).

The purpose is to catch conflicting intent before it becomes a merge conflict
in code.

### G. Dependencies

For every dependency referenced in the spec (Builds on, Related, prerequisite):

- Verify the referenced artefact exists at the stated path.
- Verify the referenced spec is in an appropriate lifecycle state.
- Verify the dependency relationship is real and material.
- Verify the current feature is not depending on unapproved or nonexistent work.

Do not allow a feature to silently depend on future work that has not been
approved. If a dependency is not ready: **BLOCK**, or explicitly record the
feature as conditionally blocked pending the dependency.

---

## 5. Plan Review Within Gate 1

Once a spec has passed the required human review, the derived plan must also
be checked before task generation proceeds.

The plan review verifies:

- The plan is derived from the approved spec.
- The plan follows the constitution.
- Integration points are named.
- Relevant data model changes are identified.
- Architectural decisions requiring ADRs are identified.
- Explicit deferrals are documented.
- The plan does not introduce new product behaviour.
- The plan does not bypass approved scope.

**SPEC = WHAT + CORRECTNESS. PLAN = HOW.**

If the plan introduces new behaviour or redefines acceptance criteria: **BLOCK.**
The correct action is to update the governing spec (through Gate 1 again) rather
than allowing implementation assumptions to become requirements.

---

## 6. Architecture Impact Assessment

Gate 1 must identify architecture-impacting changes.

When a spec or plan introduces any of the following, assess whether an ADR
is required:

- New datastore or data persistence layer
- New service or service boundary
- Changed service boundary
- Integration changes (new external system, changed protocol)
- Major new or changed API surface
- State or data ownership changes
- Significant external dependencies
- Performance-sensitive architecture decisions
- Security-sensitive architecture decisions

Use the INT principle for ADR significance: a decision is significant when
reversing it later would cost more than a day of rework.

If a significant architectural decision is identified but no ADR exists:
**flag it as a required action before Gate 1 approval is granted** or before
plan approval proceeds.

Do not allow architecture to be inferred only during coding.

Reference: `.ai-context/architecture.md`, `.ai-context/decisions/`.

---

## 7. Gate 1 Outcomes

Gate 1 has exactly two valid outcomes:

### APPROVED

The spec (and plan, where reviewed) passes all Gate 1 dimensions.
A named human non-author reviewer records this outcome in the spec's
Gate Approvals table. The spec may now transition to `Approved` in the
lifecycle and planning may begin.

### CHANGES REQUIRED

One or more issues were found. The spec (and/or plan) must be revised before
the feature may progress. The spec remains in `In Peer Review` or transitions
to `Changes Requested`. The review must document each issue with the structured
finding format (see §8 below). After revision, the spec re-enters Gate 1 review.

Do not use:

- "Reviewed"
- "Nearly approved"
- "Looks good"
- "Ready-ish"
- "Pending feedback"

These are not valid Gate 1 outcomes.

---

## 8. Structured Finding Format

Every Gate 1 finding — blocking or non-blocking — must use this format:

```
ID:                  G1-<DIMENSION>-NNN
Section:             <affected spec section>
Severity:            BLOCKING | NON-BLOCKING
Observation:         <what the reviewer found>
Why it matters:      <implementation consequence or risk>
Evidence:            <the specific text or absence in the spec>
Required correction: <what must be changed before approval>
```

Dimension codes:

| Code  | Dimension          |
| ----- | ------------------ |
| AMB   | Ambiguity          |
| TST   | Testability        |
| SCO   | Scope              |
| API   | API Contract       |
| CON   | Constitution       |
| OVL   | Overlap            |
| DEP   | Dependencies       |
| ARC   | Architecture       |
| PLN   | Plan               |
| IDN   | Identity / Reviewer |

Examples:

```
ID:                  G1-AMB-001
Section:             Acceptance Criteria — AC3
Severity:            BLOCKING
Observation:         AC3 does not define what happens after OTP session expiry.
Why it matters:      Multiple materially different implementations are possible.
Evidence:            AC3 states "Invalidate session after 5 failures" but does
                     not define post-expiry behaviour for subsequent login attempts.
Required correction: Define explicitly: whether the counter resets after expiry,
                     whether a new session begins, and what the user sees.
```

```
ID:                  G1-TST-002
Section:             Acceptance Criteria — AC5
Severity:            BLOCKING
Observation:         AC5 states "the API should be fast."
Why it matters:      "Fast" is not testable. QA cannot determine pass/fail.
Evidence:            AC5: "The endpoint should respond quickly."
Required correction: Replace with a measurable, project-defined constraint
                     or mark as Requires decision.
```

---

## 9. Blocking vs Non-Blocking Findings

### BLOCKING — Gate 1 cannot be approved

- Ambiguous behaviour where multiple materially different implementations exist
- Missing mandatory acceptance criterion
- Vague or adjective-only acceptance criterion
- Incomplete API contract (missing request, response, status codes, or errors)
- Required API exception behaviour undefined
- Constitutional conflict
- Missing or invalid dependency (referenced spec does not exist or is not Approved)
- Overlapping existing spec with conflicting ownership
- Undefined critical decision written as a fact
- Implementation detail masquerading as feature intent
- Missing required reviewer (author = reviewer)
- Missing BRD linkage
- Scope combining multiple independently deliverable features
- Plan introducing new product behaviour not in the spec

### NON-BLOCKING — Note for improvement, does not block approval

- Wording improvements that do not affect interpretation
- Formatting and readability improvements
- Minor reference cleanup (e.g., pointing to a more specific section)
- Non-material readability suggestions

Do not downgrade a correctness issue merely because fixing it is inconvenient.

---

## 10. Turnaround Expectation

Based on INT SDD guidance:

- **Target:** same working day for a spec with 5 or fewer acceptance criteria.
- **Outer bound:** 48 hours before escalation rather than indefinite delay.

This is a review-governance expectation, not a technically enforced SLA.

If no person or service is available to enforce elapsed-time escalation
automatically, the expectation is documented here and governed by the
responsible human role. Do not simulate automated SLA enforcement.

---

## 11. Revision and Traceability After Changes Requested

When Gate 1 returns `CHANGES REQUESTED`:

- The spec must be revised to address all BLOCKING findings.
- The revision history must remain visible — do not silently overwrite.
- The revision marker in the spec must be updated.
- The spec re-enters peer review (`In Peer Review` state) after revision.
- The Gate 1 reviewer assesses the revised spec, not the original.

Example revision flow:

```
Draft v1.0
  → Gate 1: Changes Requested (findings G1-AMB-001, G1-TST-002)
  → Draft v1.1 (addresses G1-AMB-001, G1-TST-002)
  → Gate 1 re-review
  → Approved
```

Do not create a parallel version numbering system if the repository already
uses Git revisions as the version record. Use the spec's `## Revision` field
to track the version marker within the artefact.

---

## 12. Automated Review vs Human Approval — Explicit Distinction

| Event                                                          | Meaning                          |
| -------------------------------------------------------------- | -------------------------------- |
| `spec-review.md` produces **READY FOR GATE 1**                 | Automated pre-submission checks passed |
| Gate 1 reviewer runs `gate-1-review.md` and all checks pass   | Reviewer analysis is complete    |
| Named human non-author records **APPROVED** in spec table     | **Gate 1 is approved**           |

These are three separate events. Only the third constitutes Gate 1 approval.

Codex/Claude running `gate-1-review.md` and finding no blocking issues does NOT
constitute Gate 1 approval.

---

## 13. Status Integration

Gate 1 outcomes must integrate with `.ai-context/status.md` and the spec's
lifecycle state:

| Gate 1 Event                   | Spec State Transition                                 |
| ------------------------------ | ----------------------------------------------------- |
| Spec submitted for peer review | `Draft` → `In Peer Review`                           |
| Gate 1: Changes Requested      | `In Peer Review` → `Changes Requested`               |
| Spec revised after feedback    | `Changes Requested` → `In Peer Review`               |
| Gate 1: Approved               | `In Peer Review` → `Approved`                        |

Do not update the spec status merely because the gate-1-review.md workflow ran.
Status transitions require the recorded human Gate 1 outcome.

Update `.ai-context/status.md` on the same day as each lifecycle transition.

---

## 14. Gate 1 vs Gate 2 — Scope Boundary

Gate 1 reviews: intent, correctness, contract, scope, feasibility, architecture
implications, and dependencies — **before implementation begins.**

Gate 2 reviews: the actual implementation diff, actual tests, actual security
posture, actual documentation, and implementation correctness — **after
implementation.**

Do not use Gate 2 as a substitute for a weak Gate 1. Issues that should be
caught at Gate 1 must not be deliberately deferred to Gate 2.

If Gate 2 uncovers what is actually a spec-level defect, the correct action is
to return work to the governing artefact (spec or BRD) — not to patch the
implementation.
