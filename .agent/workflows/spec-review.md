# Specification Review Workflow

## Authority

Validate a feature specification before it enters Gate 1 peer review.
The governing standard is `.agent/rules/int-sdd-specification-standard.md`.
Run `.agent/workflows/spec-prewriting-gate.md` before authoring; run this workflow
before submitting for Gate 1.

## Inputs Required

- `.ai-context/specs/<feature-slug>.spec.md`
- Linked `.ai-context/BRD.md#BRD-NNN`
- `.ai-context/constitution.md`
- `.ai-context/architecture.md` (relevant sections)
- Any referenced ADRs under `.ai-context/decisions/`
- Any referenced related specs under `.ai-context/specs/`

---

## Spec Quality Gate Checklist

Every item below must pass before the spec is `READY FOR GATE 1`.
If any **[BLOCKING]** item fails, the spec is **NOT READY**.

### Scope and Identity

- [ ] **[BLOCKING]** One feature only — not a bundle of independent features,
      not a project-wide spec, not a vague improvement.
- [ ] **[BLOCKING]** Unique, specific, human-readable kebab-case `<feature-slug>`.
      Not a generic ID (`feature-42`, `misc-change`, `new-feature`).
- [ ] Status is set to a valid lifecycle state from `.ai-context/lifecycle.md`.

### BRD Linkage

- [ ] **[BLOCKING]** A validated `BRD-NNN` entry exists in `.ai-context/BRD.md`
      and the spec's Linked BRD field references it.
- [ ] The spec's intent does not materially diverge from the BRD entry.

### Intent

- [ ] **[BLOCKING]** Intent is one concise, unambiguous paragraph.
- [ ] **[BLOCKING]** Intent states: what changes, for whom, under what condition,
      and the expected observable outcome.
- [ ] Intent is specific enough that two competent engineers cannot build
      materially different behaviour.
- [ ] Intent contains no vague adjectives as substitutes for behaviour
      ("secure", "fast", "robust", "good", "improved").

### Context and Dependencies

- [ ] Context links authoritative artefacts — no large blocks of repository
      context duplicated into the spec.
- [ ] Every referenced related spec is real, exists in the repository, and has
      a usable lifecycle status.
- [ ] Unresolved dependencies are explicitly flagged — not silently assumed.

### API Contract (when applicable)

- [ ] **[BLOCKING when API surface exists]** API Contract section is present and
      uses stable `<feature-slug>.APINN` identifiers.
- [ ] **[BLOCKING when API surface exists]** Method and path are defined.
- [ ] **[BLOCKING when API surface exists]** Request payload shape is defined.
- [ ] **[BLOCKING when API surface exists]** Success response status code and
      body shape are defined.
- [ ] **[BLOCKING when API surface exists]** Exception/error conditions and
      response shapes are defined.
- [ ] **[BLOCKING when API surface exists]** Relevant validation behaviour is
      defined.
- [ ] When no API surface: the section explicitly states
      `Not applicable — no API surface is specified.`
- [ ] No product-level API behaviour is deferred to the plan.

### Acceptance Criteria

- [ ] **[BLOCKING]** At least one acceptance criterion exists.
- [ ] **[BLOCKING]** Every AC has a stable `<feature-slug>.ACN` identifier.
- [ ] **[BLOCKING]** Every AC is objectively observable and testable — not
      stated as a capability or aspiration.
- [ ] Given/When/Then structure is used where the behaviour can be expressed
      in that form.
- [ ] No AC uses vague adjectives in place of observable behaviour:
      "secure", "fast", "robust", "good", "works correctly", "properly".

### Failure Paths

- [ ] Invalid input behaviour is defined or explicitly flagged as unresolved.
- [ ] Missing required input behaviour is defined or flagged.
- [ ] Unauthorized access is addressed or flagged.
- [ ] Authentication failure is addressed or flagged.
- [ ] Validation failure is addressed or flagged.
- [ ] Duplicate request behaviour is addressed or flagged where relevant.
- [ ] Boundary values are addressed or flagged where relevant.
- [ ] Error / exception states are addressed or flagged.
- [ ] Partial failure behaviour is addressed or flagged where relevant.
- [ ] Timeout / failure behaviour is addressed or flagged where relevant.
- [ ] Important null / undefined conditions are addressed or flagged.
- [ ] Any unresolved failure path is explicitly identified — not silently omitted.

### Spec-Derived Unit Test Cases

- [ ] **[BLOCKING]** Unit test case table exists with at least one row.
- [ ] **[BLOCKING]** Every unit test uses a stable `<feature-slug>.UTNN`
      identifier.
- [ ] **[BLOCKING]** Every unit test maps to an AC identifier.
- [ ] Tests derive from the spec (Spec → AC → Test), not from code.
- [ ] Acceptance-derived tests are distinct from broader QA concerns (which
      belong in `.ai-context/test_cases/<feature-slug>.test_cases.md`).

### Explicit Out of Scope

- [ ] **[BLOCKING]** The "Explicitly Out of Scope" section exists and contains
      at least one item or the explicit statement that no adjacent exclusions
      are required.
- [ ] Major adjacent behaviours that could cause scope creep are explicitly
      excluded.

### Non-Functional Constraints

- [ ] Non-functional constraints that apply are referenced with their
      authoritative source (constitution, approved decision, BRD).
- [ ] No invented numbers or aspirational targets without an authoritative
      source.
- [ ] Unresolved non-functional requirements are marked `Requires decision`.

### Open Items and Decisions Required

- [ ] All unresolved items are listed in the Open Items section.
- [ ] No unresolved decision has been silently converted into a stated fact
      in the spec body.

### Safety

- [ ] No secrets, passwords, API keys, tokens, credentials, PII, production
      data, or confidential customer information is present.
- [ ] Synthetic placeholders are used where representative data is needed.

### Boundary — No Implementation Leakage

- [ ] **[BLOCKING]** The spec does not contain an implementation plan disguised
      as requirements.
- [ ] **[BLOCKING]** No framework choices, class names, database structures,
      internal algorithms, source layout, library selection, or call sequences
      that are not approved contractual constraints.
- [ ] No task list, coding prompt, or architecture dump is present.
- [ ] Speculative technical choices are not encoded as feature intent.

### Gate Approvals

- [ ] Gate Approvals table is present.
- [ ] Gate 1 reviewer field is populated or marked pending.

---

## Anti-Pattern Block

The following anti-patterns automatically constitute a **CHANGES REQUIRED** result.
Check each before submitting.

| Anti-pattern                                                    | Check |
| --------------------------------------------------------------- | ----- |
| Multiple independent features inside one spec                   | [ ]   |
| Giant project-wide or module-wide specification                 | [ ]   |
| Vague or adjective-only intent                                  | [ ]   |
| Acceptance criteria without stable IDs                          | [ ]   |
| "Secure", "fast", "robust", "good" as acceptance criteria       | [ ]   |
| Requirements hidden inside technical implementation prose        | [ ]   |
| Missing API contract when an API surface exists                 | [ ]   |
| API payload omitted                                             | [ ]   |
| API success response omitted                                    | [ ]   |
| API exception conditions omitted                                | [ ]   |
| Happy-path-only acceptance criteria                             | [ ]   |
| Missing Explicitly Out of Scope section                         | [ ]   |
| Invented requirements with no BRD source                        | [ ]   |
| Unresolved decisions silently written as stated facts           | [ ]   |
| Spec copied from code — code treated as authoritative definition| [ ]   |
| Large blocks of unrelated repository context pasted into spec   | [ ]   |
| Implementation details forced into spec as feature intent       | [ ]   |
| Missing stable AC IDs                                           | [ ]   |
| Missing BRD linkage                                             | [ ]   |
| Dependencies not identified                                     | [ ]   |
| Fake, meaningless, or placeholder-only spec content             | [ ]   |

---

## Spec Change Propagation Check

When reviewing a revised spec (not an initial draft), check whether the changes
are material:

**Material change** = a change that alters intended behaviour, acceptance criteria,
API contract, scope, or out-of-scope boundaries.

If the spec has changed materially after planning began, identify which downstream
artefacts may be stale:

- [ ] Plan (`.ai-context/plans/<feature-slug>.plan.md`) — does it still derive
      from the updated spec?
- [ ] Tasks (`.ai-context/tasks/<feature-slug>.tasks.md`) — do tasks still map
      to valid AC/API IDs?
- [ ] Test cases (`.ai-context/test_cases/<feature-slug>.test_cases.md`) — do
      tests still map to current AC IDs?
- [ ] Executable tests — are there now stale or missing tests?
- [ ] Implementation — is any in-progress or completed implementation now
      inconsistent with the updated spec?

**Do not automatically rewrite downstream artefacts.** First identify which are
affected, then require explicit human decision before updating them.

---

## Gate 1 Readiness Criteria

A Gate 1 reviewer should be able to inspect and answer:

| Question                                                         | Expected answer |
| ---------------------------------------------------------------- | --------------- |
| Could two engineers build materially different behaviour?         | No              |
| Can every AC actually be tested?                                 | Yes             |
| Is the feature bounded (one feature, one spec)?                  | Yes             |
| Is the contractual API behaviour complete (where applicable)?    | Yes             |
| Are related artefacts real and usable?                           | Yes             |
| Are applicable constitution/project constraints represented?     | Yes             |

---

## Outcome

Report exactly one of:

### READY FOR GATE 1

Every required checklist item passes. List the evidence for each blocking check.
If non-blocking TBD items are accepted, list them explicitly.

**Reminder:** A `READY FOR GATE 1` result is **not Gate 1 approval.** Approval
requires the named human non-author review recorded in the spec's Gate Approvals
table per `.ai-context/lifecycle.md`.

### CHANGES REQUIRED

List each blocking issue using this structure:

```
Section: <affected section>
Issue: <what is wrong>
Impact: <why it blocks Gate 1>
Required correction: <what must change>
```

---

## Self-Approval Prohibition

Codex/Claude must not declare its own specification approved.

A workflow result of `READY FOR GATE 1` means the automated checks pass.
Human Gate 1 approval is a separate, mandatory governance event.
