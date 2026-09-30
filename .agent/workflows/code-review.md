# Code Review Workflow

## Authority

This is the task-scoped review workflow for INT SDD implementation review.
It is used at Step 8 of `.agent/workflows/task-by-task-development.md`
as the pre-commit quality gate, and as supporting evidence for Gate 2.

Governing rule: `.agent/rules/int-sdd-development.md` §6
Non-negotiables: `.agent/rules/int-sdd-non-negotiables.md`
Development discipline check: `.agent/workflows/development-discipline-check.md` Part 3
Lifecycle: `.ai-context/lifecycle.md` (Gate 2 is the pre-merge human review)

This workflow governs task-level review before commit.
Gate 2 (`.ai-context/lifecycle.md`) governs the broader human review before merge.
These are distinct gates. Do not conflate them.

---

## Inputs Required

- Task ID: `<feature-slug>.TNN`
- Feature slug: `<feature-slug>`
- Approved spec: `.ai-context/specs/<feature-slug>.spec.md`
- Approved plan: `.ai-context/plans/<feature-slug>.plan.md`
- Tasks file: `.ai-context/tasks/<feature-slug>.tasks.md`
- Generated changeset / diff for this task
- Test results (RED confirmed before implementation; GREEN confirmed after)

---

## The Task-Scoped Review Question

The review question is NOT:

> "Does the whole feature look okay?"

It IS:

> "Does this generated change correctly implement `<slug>.TNN` within the
> approved specification, plan, architecture, and acceptance criteria?"

Every review finding must be traced to the task ID and its contract.

---

## Review Procedure

### 1. Task Identity Confirmation

- [ ] **[BLOCKING]** The change being reviewed is for task `<feature-slug>.TNN`.
- [ ] **[BLOCKING]** The task ID exists in `.ai-context/tasks/<feature-slug>.tasks.md`.
- [ ] **[BLOCKING]** The spec status is `Approved`.
- [ ] **[BLOCKING]** Gate 1 was completed with a named human non-author reviewer.

### 2. Acceptance Criteria Verification

For each AC ID in the task's traceability column:

- [ ] **[BLOCKING]** AC `<slug>.AC1` — state how this is satisfied by the changeset.
- [ ] **[BLOCKING]** AC `<slug>.AC2` — state how this is satisfied by the changeset.
- [ ] Each AC is individually verified, not assumed satisfied as a group.
- [ ] No AC was silently excluded from the implementation.

Record per AC:

| AC ID | Satisfied? | Evidence | Notes |
| ----- | ---------- | -------- | ----- |
| `<slug>.AC1` | `Yes / No / Partial` | `<test result or observable>` | |

### 3. API Contract Verification (if applicable)

- [ ] **[BLOCKING when applicable]** Each `<slug>.APINN` referenced by this task
      is implemented exactly as specified in the approved spec.
- [ ] No request/response field, status code, or error behaviour was invented
      beyond the approved contract.
- [ ] State `Not applicable — no API surface for this task` if this task has no API surface.

### 4. Test-First Evidence

- [ ] **[BLOCKING]** RED evidence was confirmed before implementation:
      test command and failure output are recorded.
- [ ] **[BLOCKING]** All relevant test cases now pass (GREEN).
- [ ] No test was weakened, removed, or had its expected outcome changed to pass.
- [ ] Each passing test maps to a valid, non-retired AC ID.

Test evidence record:

| Test ID | Mapped to AC | RED confirmed | GREEN confirmed | Notes |
| ------- | ------------ | ------------- | --------------- | ----- |
| `<slug>.UT01` | `<slug>.AC1` | `Yes / No` | `Yes / No` | |

### 5. Security and Constitution

- [ ] **[BLOCKING]** No security constraint from the spec or constitution is violated.
- [ ] **[BLOCKING]** No secret, credential, API key, PII, or sensitive data is present
      in any changed file, test fixture, configuration, or comment.
- [ ] Security-relevant spec behaviour is implemented or explicitly noted as
      outside this task's scope with a reference to the task that covers it.

### 6. Architecture and Dependency Compliance

- [ ] **[BLOCKING]** No architectural change is present beyond what the approved plan describes.
- [ ] **[BLOCKING]** No unregistered dependency was introduced.
- [ ] **[BLOCKING]** No undeclared external API, data store, service, or integration was added.
- [ ] Relevant ADRs referenced by the plan are respected.

### 7. Deferred Scope

- [ ] **[BLOCKING]** No item from the plan's Deferred Implementation Items is
      present in this changeset.

### 8. Changeset Scope and Unexpected Files

- [ ] **[BLOCKING if unexplained]** All changed files are within the task's
      declared scope boundary.
- [ ] No unrelated refactoring is mixed into this changeset.
- [ ] No opportunistic cleanup beyond what the task requires.
- [ ] No formatting-only changes in files unrelated to the task.
- [ ] If unexpected files are present: each is individually justified as
      necessary for this specific task.

### 9. Behaviour Outside the Spec

- [ ] **[BLOCKING]** The changeset does not introduce behaviour that was not
      specified in the approved spec.
- [ ] No inference or assumption of unstated product behaviour was made.

### 10. Unrelated Changes

- [ ] **[BLOCKING]** The changeset does not contain implementation work
      from another feature or task.
- [ ] Commit does not silently include changes to future task scope.

### 11. Repository Standards and Traceability

- [ ] **[BLOCKING]** Commit message does NOT contain AI attribution language
      (`Generated by Claude`, `AI-assisted`, `Written by AI`, etc.).
- [ ] Commit message references the task ID in traceable form
      (`Implements <slug>.TNN`).
- [ ] Line 9 naming and ID consistency is maintained:
      no ID drift, no malformed IDs, no cross-feature reference without governance.

### 12. Human Accountability

- [ ] A named human engineer or reviewer has examined this changeset.
- [ ] AI generation alone does not constitute review.
- [ ] Passing tests alone do not constitute review.
- [ ] Gate 2 remains pending for merge authorization.

---

## Review Outcome

Classify the review outcome:

| Classification | Condition | Action |
| -------------- | --------- | ------ |
| `Correct` | All checks pass; change satisfies the task contract | Commit (Step 10 of development workflow) |
| `Partially correct` | Local implementation error; correctable within task scope | Correct locally; re-run this review |
| `Significantly wrong` | Fundamental misunderstanding of requirement or contract violation | STOP; classify failure source per int-sdd-development.md §7.2 |
| `Blocked by upstream` | Ambiguity, conflict, or missing requirement in authoritative artefacts | STOP; route upstream; do not continue implementation |

---

## Review Record

Record the following for each task review:

```
Task ID:             <feature-slug>.TNN
AC IDs verified:     [list]
API IDs verified:    [list or N/A]
Files changed:       [list]
Tests run:           [test IDs and results]
Review outcome:      [Correct / Partially correct / Significantly wrong / Blocked]
Issues found:        [list or None]
Corrective action:   [list or None]
Reviewer/owner:      <name or role>
Commit reference:    [pending / <commit hash>]
Date:                YYYY-MM-DD
```

---

## Completion Signal

A task review is complete when:

1. Review outcome is classified as `Correct`.
2. All BLOCKING items above pass.
3. The review record is complete.
4. Human accountability is established for this review.

**This workflow produces task-level quality evidence — not merge authorization.**
Gate 2 (human review before merge) remains a separate required event.
