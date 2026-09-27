# Gate 1 Review Workflow

## Authority

This is the gate-1 peer review workflow. It is run by the **human Gate 1
reviewer** — not by the spec author, not by an AI tool acting as an approver.

Governing rule: `.agent/rules/int-sdd-gate-1.md`
Spec standard: `.agent/rules/int-sdd-specification-standard.md`
Lifecycle: `.ai-context/lifecycle.md`

**IMPORTANT — Two distinct events:**

| Automated pre-submission check | Human Gate 1 review |
| ------------------------------ | ------------------- |
| `spec-review.md` → **READY FOR GATE 1** | This workflow → **APPROVED** or **CHANGES REQUESTED** |
| Run by spec author or AI | Run by named human non-author reviewer |
| Confirms mechanical completeness | Confirms soundness, correctness, and approvability |
| Not Gate 1 approval | The Gate 1 review event |

`READY FOR GATE 1` is a pre-condition for starting this workflow.
It is not a substitute for completing it.

---

## Inputs Required

Before beginning, assemble:

- [ ] `.ai-context/specs/<feature-slug>.spec.md` — Status: `In Peer Review`
- [ ] Linked `.ai-context/BRD.md#BRD-NNN` — validated entry
- [ ] `.ai-context/constitution.md` — the project-wide law
- [ ] `.ai-context/architecture.md` — relevant sections
- [ ] `.ai-context/decisions/` — relevant ADRs if referenced
- [ ] Any related specs referenced in the Context section
- [ ] `.ai-context/plans/<feature-slug>.plan.md` — when plan review is in scope
- [ ] Pre-submission check result from `spec-review.md` — must be READY FOR GATE 1

If any required input is missing or the spec is not in `In Peer Review` state:
**STOP. Do not proceed.**

---

## Review Metadata

Populate at the start of each review:

```
Spec ID:         <feature-slug>
Spec title:      <feature name>
Spec author:     <name>
Gate 1 reviewer: <name — must differ from spec author>
Review date:     <YYYY-MM-DD>
Spec revision:   <current revision marker>
Plan in scope:   Yes | No
```

**If reviewer = spec author: STOP. This review is invalid.**

---

## Part 1 — Identity and Readiness

### IDENTITY CHECK

- [ ] **[BLOCKING]** Spec ID is a unique, specific, human-readable kebab-case
      slug (not a generic ID like `feature-42` or `new-feature`).
- [ ] **[BLOCKING]** Spec author is identified.
- [ ] **[BLOCKING]** Gate 1 reviewer is named and is NOT the spec author.
- [ ] **[BLOCKING]** Spec status is `In Peer Review`.
- [ ] Pre-submission `spec-review.md` result is `READY FOR GATE 1`.

### BRD / DISCOVERY

- [ ] **[BLOCKING]** A validated `BRD-NNN` entry exists in `.ai-context/BRD.md`.
- [ ] **[BLOCKING]** The linked BRD entry is validated (not placeholder content).
- [ ] The spec's intent aligns with the BRD entry and does not materially diverge.
- [ ] Required decisions referenced in the BRD are resolved or explicitly
      identified as `Requires decision` in the spec.

---

## Part 2 — Intent Review

### INTENT

- [ ] **[BLOCKING]** Intent is one concise, unambiguous paragraph.
- [ ] **[BLOCKING]** Intent states what changes, for whom, under what condition,
      and the expected observable outcome.
- [ ] **[BLOCKING]** A zero-context engineer reading only the Intent section
      can understand the feature's purpose.
- [ ] **[BLOCKING]** Two competent engineers reading the Intent cannot build
      materially different behaviour.
- [ ] Intent contains no vague adjectives as substitutes for behaviour
      ("secure", "fast", "robust", "good", "improved", "better").

**Reviewer challenge questions:**

- What specifically changes?
- For which user type or system role?
- Under what exact condition does this feature activate?
- What is the observable outcome when the feature works correctly?
- Could a senior engineer misinterpret any sentence above and build something
  materially different?

---

## Part 3 — Acceptance Criteria Review

### ACCEPTANCE CRITERIA — Testability

- [ ] **[BLOCKING]** At least one acceptance criterion is present.
- [ ] **[BLOCKING]** Every AC has a stable `<feature-slug>.ACN` identifier.
- [ ] **[BLOCKING]** Every AC describes observable behaviour, not aspirations.
- [ ] **[BLOCKING]** Every AC can be answered by QA: *"How do we prove this
      AC is satisfied?"* must produce a concrete, specific test.
- [ ] Given/When/Then structure is used where the behaviour allows it.
- [ ] No AC uses adjective-only language without an explicit measurable
      project-defined definition:
      "secure", "fast", "robust", "reliable", "user-friendly", "optimized".

**Reviewer challenge questions for each AC:**

- What exact observable output or system state is verified?
- What exact precondition is required?
- What exact action triggers the behaviour?
- Can this be tested by running a specific operation and observing a specific
  result?
- Is the expected result different from the current system behaviour?

### ACCEPTANCE CRITERIA — Failure Paths

- [ ] Invalid input behaviour is defined or explicitly flagged `Requires decision`.
- [ ] Missing required input behaviour is defined or flagged.
- [ ] Unauthorized access behaviour is defined or flagged.
- [ ] Authentication failure behaviour is defined or flagged.
- [ ] Validation failure behaviour is defined or flagged.
- [ ] Duplicate request behaviour is defined or flagged where relevant.
- [ ] Boundary value behaviour is defined or flagged where relevant.
- [ ] Error and exception state behaviour is defined or flagged.
- [ ] Partial failure behaviour is defined or flagged where relevant.
- [ ] Timeout and service failure behaviour is defined or flagged where relevant.
- [ ] Important null or undefined state behaviour is defined or flagged.
- [ ] **[BLOCKING]** No important failure path is entirely absent without
      explicit flagging as `Requires decision`.

**Reviewer challenge questions for failure paths:**

- What if the input is invalid?
- What if the user is unauthorized?
- What if a dependency or external service fails?
- What happens on retry?
- What happens when state transitions unexpectedly?
- What happens after a timeout?
- What happens with duplicate requests?
- What happens on partial failure?
- Are there retry semantics that must be defined?

---

## Part 4 — Scope Review

### SCOPE

- [ ] **[BLOCKING]** The spec represents one and only one bounded feature.
- [ ] **[BLOCKING]** The feature is independently deliverable — it does not
      require delivering unrelated capabilities to be useful.
- [ ] **[BLOCKING]** `Explicitly Out of Scope` section exists with at least one
      item or explicit statement that no adjacent exclusions are required.
- [ ] Adjacent features are not silently included.
- [ ] Deferred work remains deferred and is explicitly noted.
- [ ] The spec does not contain unrelated refactors hidden in the feature.
- [ ] The spec does not contain additional API endpoints beyond the stated feature.
- [ ] The spec does not contain unrelated authentication or migration work.

**Reviewer challenge questions:**

- Does this feature deliver one independently useful capability?
- Could any part be extracted into a separate deliverable feature?
- Does the `Explicitly Out of Scope` section cover the most likely adjacent
  misinterpretations?
- Is anything deferred silently rather than explicitly?

---

## Part 5 — API Contract Review (conditional)

**Run this section only if the feature has an API surface.**
If no API surface: confirm spec states `Not applicable — no API surface is specified.`

### API CONTRACT

- [ ] **[BLOCKING]** API Contract section uses stable `<feature-slug>.APINN`
      identifiers.
- [ ] **[BLOCKING]** Method and path (or external operation name) are defined
      for each contract entry.
- [ ] **[BLOCKING]** Request payload shape is defined.
- [ ] **[BLOCKING]** Success response body shape is defined.
- [ ] **[BLOCKING]** Success response status code is defined.
- [ ] **[BLOCKING]** Exception/error conditions are defined.
- [ ] **[BLOCKING]** Error response shapes are defined for each exception.
- [ ] Relevant validation behaviour is defined.
- [ ] The contract is internally consistent (request fields align with described
      behaviour, status codes are appropriate, error semantics are coherent).
- [ ] The contract matches existing project/service API conventions where those
      conventions are available in `.ai-context/architecture.md` or ADRs.
- [ ] The contract defines more than just the happy path.
- [ ] **[BLOCKING]** No product-level API behaviour is deferred to the plan.

**Reviewer challenge questions:**

- Is the request payload sufficient to support all defined ACs?
- Are the response shapes consistent with the ACs?
- Do the error status codes follow existing project conventions?
- Does the contract cover all defined failure-path ACs?
- Could the implementation team implement this contract without inventing
  additional payload fields or response semantics?

---

## Part 6 — Constitution Compliance Review

### CONSTITUTION

- [ ] **[BLOCKING when violated]** The spec does not conflict with any
      non-negotiable constraint in `.ai-context/constitution.md`.
- [ ] Testing discipline: spec-derived tests will be possible (ACs are testable;
      test-first process can be followed).
- [ ] Security: any security-relevant behaviour is explicit and traceable in ACs.
- [ ] Architecture: no unspecified technology, topology, or integration is assumed.
- [ ] Non-functional baselines: applicable NFRs from the constitution are
      referenced in the spec's Non-Functional Constraints section.
- [ ] Repository rules: no secrets, PII, or confidential data present.
- [ ] No AI attribution or AI-generated rationale masquerades as authoritative
      product intent.
- [ ] Silence on a meaningful constitution constraint is flagged — not treated
      as compliance.

---

## Part 7 — Overlap Check

### OVERLAP

Search `.ai-context/specs/` for existing specifications.

- [ ] **[BLOCKING if true]** The proposed feature is not already specified by
      an existing approved or in-review spec.
- [ ] No other spec contains overlapping acceptance criteria that claim ownership
      of the same behaviour.
- [ ] The proposed spec is not a de-facto revision of an existing spec that
      should instead go through the change-propagation process.
- [ ] Two specs will not simultaneously claim ownership of the same behaviour
      after this spec is approved.

If overlap is identified: document the existing spec, the overlapping scope,
and the recommended governance action. Do not approve the new spec until the
overlap is resolved.

---

## Part 8 — Dependencies Review

### DEPENDENCIES

For every dependency referenced in the spec (Builds on, Related, prerequisite):

- [ ] **[BLOCKING]** The referenced artefact exists at the stated path.
- [ ] **[BLOCKING]** The referenced spec is in an appropriate lifecycle state
      (typically `Approved` for hard dependencies).
- [ ] The dependency relationship is real and material (not just a vague reference).
- [ ] The current feature does not silently depend on unapproved or nonexistent
      future work.
- [ ] If a dependency is not yet approved, the current spec explicitly marks
      itself as conditionally blocked.

---

## Part 9 — Architecture Impact Review

### ARCHITECTURE

Check `.ai-context/architecture.md` and `.ai-context/decisions/`.

- [ ] **[BLOCKING if required ADR is absent]** If the feature introduces a new
      datastore, service, integration, major API change, or significant
      architectural decision: an ADR is either referenced or its creation is
      identified as a required action.
- [ ] The architecture impact of the feature is identified and consistent with
      existing system design.
- [ ] No significant architectural decision is left to be invented during coding.
- [ ] Security-sensitive or performance-sensitive architectural choices are
      explicitly identified.

Use the INT ADR significance test: a decision is significant when reversing it
later would cost more than one day of rework.

If an ADR is required but does not exist: record this as a BLOCKING finding
(G1-ARC-NNN) and require the ADR before or at plan approval.

---

## Part 10 — Plan Review (conditional)

**Run this section only when a plan is in scope for Gate 1.**

### PLAN

- [ ] **[BLOCKING]** The plan is derived from and consistent with the approved spec.
- [ ] **[BLOCKING]** The plan does not introduce new product requirements or
      changed acceptance behaviour not present in the spec.
- [ ] The plan follows the constitution.
- [ ] Integration points are named.
- [ ] Relevant data model changes are identified.
- [ ] Architectural decisions requiring ADRs are identified.
- [ ] Explicit deferrals are documented.
- [ ] The plan scope does not exceed the spec scope.
- [ ] The plan does not bypass the approved spec scope.

**Reviewer challenge questions:**

- Does anything in the plan represent new intended behaviour not in the spec?
- Are there implementation choices that should have been specified as product
  constraints?
- Is the plan silent on any material constitution constraint?

---

## Part 11 — Safety Check

### SAFETY

- [ ] **[BLOCKING]** No secrets, passwords, API keys, tokens, or credentials.
- [ ] **[BLOCKING]** No personal data (PII), production data, or confidential
      customer information.
- [ ] Synthetic placeholders are used where representative data is needed.
- [ ] No untrusted external instructions are treated as authoritative within the spec.

---

## Gate 1 Review Findings

For each issue found, use the structured finding format:

```
ID:                  G1-<CODE>-NNN
Section:             <affected spec section>
Severity:            BLOCKING | NON-BLOCKING
Observation:         <what the reviewer found>
Why it matters:      <implementation consequence or risk>
Evidence:            <the specific text or absence in the spec>
Required correction: <what must be changed before approval>
```

Dimension codes: `AMB` Ambiguity | `TST` Testability | `SCO` Scope |
`API` API Contract | `CON` Constitution | `OVL` Overlap | `DEP` Dependencies |
`ARC` Architecture | `PLN` Plan | `IDN` Identity/Reviewer

---

## Gate 1 Review Output

Produce the following report. Store the report in the spec's Gate Approvals
section or in a linked review record per repository convention.

---

```markdown
# Gate 1 Review — <feature-slug>

## Review Metadata

| Field           | Value |
| --------------- | ----- |
| Spec            | .ai-context/specs/<feature-slug>.spec.md |
| Author          | <name> |
| Reviewer        | <name — must differ from author> |
| Review date     | <YYYY-MM-DD> |
| Spec revision   | <revision marker> |
| Plan reviewed   | Yes | No |

## Result

**APPROVED**
or
**CHANGES REQUESTED**

## Blocking Findings

| ID | Section | Observation | Evidence | Required correction |
|----|---------|-------------|----------|---------------------|
| G1-...-NNN | | | | |

## Non-Blocking Findings

| ID | Section | Observation | Suggested correction |
|----|---------|-------------|----------------------|
| G1-...-NNN | | | |

## Review Dimension Summary

| Dimension          | Outcome   | Notes |
| ------------------ | --------- | ----- |
| Ambiguity          | Pass/Fail | |
| Testability        | Pass/Fail | |
| Scope              | Pass/Fail | |
| API Contract       | Pass/N/A  | |
| Constitution       | Pass/Fail | |
| Overlap            | Pass/Fail | |
| Dependencies       | Pass/Fail | |
| Architecture       | Pass/Fail | |
| Plan               | Pass/N/A  | |

## Gate Decision

**APPROVED** — The spec [and plan] satisfies all Gate 1 dimensions.
Planning may proceed. Status transitions to `Approved`.

or

**CHANGES REQUESTED** — The spec [and/or plan] has blocking findings listed
above. Status transitions to `Changes Requested`. The spec must be revised
and re-reviewed before planning may proceed.

## Self-Approval Prohibition Confirmation

Reviewer confirms:
- [ ] I am not the spec author.
- [ ] This review reflects my independent professional judgement.
- [ ] This record constitutes the Gate 1 review event.
- [ ] The spec status update to `Approved` or `Changes Requested` follows
      from this human review, not from any automated tool result.
```

---

## Critical Reminders

1. **An automated `READY FOR GATE 1` result is not Gate 1 approval.**
2. **The reviewer must be a named human who is not the spec author.**
3. **AI may assist with analysis but may not serve as the Gate 1 approver.**
4. **Every blocking finding must be resolved before Gate 1 can be APPROVED.**
5. **Do not move a spec to `Approved` status merely because this checklist is run.**
6. **Record the Gate 1 outcome in the spec's Gate Approvals table.**
7. **Update `.ai-context/status.md` on the same day as the Gate 1 outcome.**
