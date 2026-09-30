# Development Discipline Check Workflow

## Authority

This workflow validates development discipline compliance for a task execution
under INT SDD Line 10.

Governing rule: `.agent/rules/int-sdd-development.md`
Execution workflow: `.agent/workflows/task-by-task-development.md`
Naming/identity: `.agent/rules/int-sdd-naming.md`
Non-negotiables: `.agent/rules/int-sdd-non-negotiables.md`

---

## When to Run

- **Part 1** (Task Readiness): Before starting or resuming implementation of a task.
- **Part 2** (Changeset Scope): After generation, before review.
- **Part 3** (Review Completeness): Before committing.
- **Part 4** (Commit Compliance): When preparing or inspecting a commit.
- **Part 5** (Status Accuracy): When updating status at any point.
- **Full check**: When auditing compliance for a feature in development.

---

## OUTPUT FORMAT

Produce either:

```
DISCIPLINE CHECK PASSED
```

or:

```
DISCIPLINE CHECK — BLOCKED: <findings>
DISCIPLINE CHECK — WARNING: <findings>
```

Use BLOCKED for issues that prevent the next step from proceeding.
Use WARNING for issues that require human review but do not halt the step.

---

## Part 1 — Task Readiness Check

_Run before Step 2 of task-by-task-development.md._

### 1.1 Task Existence and Identity

- [ ] **[BLOCKING]** Task ID `<feature-slug>.TNN` exists in
      `.ai-context/tasks/<feature-slug>.tasks.md`.
- [ ] **[BLOCKING]** Task ID follows the correct format
      (see `.agent/rules/int-sdd-naming.md` §8.1).
- [ ] **[BLOCKING]** Task ID prefix matches the feature slug exactly.
- [ ] **[BLOCKING]** Task state is `Not Started` or `In Progress` (resuming).
  - State `Merged` → task already complete; do not re-implement.
  - State `In Review` → task is pending Gate 2; do not restart unless review failed.

### 1.2 Upstream Chain

- [ ] **[BLOCKING]** Spec exists: `.ai-context/specs/<feature-slug>.spec.md`
- [ ] **[BLOCKING]** Spec status is `Approved`.
- [ ] **[BLOCKING]** Gate 1 record shows a named human non-author reviewer and `Approved` outcome.
- [ ] **[BLOCKING]** Plan exists: `.ai-context/plans/<feature-slug>.plan.md`
- [ ] **[BLOCKING]** Plan has passed the Architecture Check
      (status: `Architecture Check Passed`).
- [ ] **[BLOCKING]** Tasks file exists: `.ai-context/tasks/<feature-slug>.tasks.md`

### 1.3 Task Traceability

- [ ] **[BLOCKING]** Task's traceability column references at least one AC ID.
- [ ] **[BLOCKING]** Each referenced AC ID (`<slug>.ACN`) exists in the spec
      and is not retired.
- [ ] **[BLOCKING]** Each referenced API ID (`<slug>.APINN`) exists in the spec
      and is not retired (if applicable).
- [ ] **[BLOCKING]** No referenced ID uses an incorrect slug prefix.

### 1.4 Task Clarity and Boundary

- [ ] **[BLOCKING]** Task description is specific enough to implement without
      guessing a missing business requirement.
- [ ] **[BLOCKING]** Task scope boundary explicitly names in-scope modules and
      explicit exclusions.
- [ ] **[BLOCKING]** Task does not include work from another task without explicit
      governance (e.g., combined task defined in the plan).

### 1.5 Dependency Status

- [ ] **[BLOCKING if ordering is mandatory]** All prior dependent tasks that must
      complete before this task are in state `Merged`.
- [ ] No unresolved architectural decision is required before this task can proceed.
  - If unresolved ADR: **BLOCKED** — resolve through `adr-check.md` first.

### 1.6 Test-First Prerequisites

- [ ] **[BLOCKING]** Test cases for this task's AC IDs exist in
      `.ai-context/test_cases/<feature-slug>.test_cases.md`.
- [ ] **[BLOCKING]** Executable tests for this task's ACs are created and reviewed.
- [ ] **[BLOCKING]** RED evidence is confirmed:
      test command + failing output recorded before any implementation.

**Part 1 result:**

```
TASK READY: <feature-slug>.TNN
```

or:

```
TASK NOT READY: <feature-slug>.TNN
Blocking conditions:
- [condition 1]
- [condition 2]
```

---

## Part 2 — Changeset Scope Check

_Run after generation (Step 7 of task-by-task-development.md), before review._

### 2.1 File Scope Triage

For each file in the generated diff:

- [ ] **[BLOCKING if any file fails]** Every changed file is explainable as
      necessary to satisfy `<feature-slug>.TNN` and its approved contract.

For each file that appears unexpected:

| File | In task scope boundary? | Explainable as task-necessary? | Decision |
| ---- | ----------------------- | ------------------------------ | -------- |
| `<path>` | `Yes / No` | `Yes / No` | `Accept / FLAG` |

- [ ] **[BLOCKING]** No file belongs to a different feature (different slug prefix path or explicitly different domain module).
- [ ] **[BLOCKING]** No file implements deferred scope items listed in the plan.
- [ ] **[WARNING]** No file appears to belong to a different task in the same feature.
- [ ] **[WARNING]** No apparent refactoring of code unrelated to this task.
- [ ] **[WARNING]** No opportunistic cleanup unrelated to this task.
- [ ] **[WARNING]** No dependency additions not required by this task.

### 2.2 Changeset Test

Answer explicitly:

```
CAN THE ENTIRE DIFF BE TRACED TO <feature-slug>.TNN AND ITS APPROVED CONTRACT?
```

- `YES` → proceed to review.
- `NO` → identify out-of-scope changes; stop and classify.

**Part 2 result:**

```
SCOPE CHECK PASSED: <feature-slug>.TNN — all changed files are task-traceable
```

or:

```
SCOPE VIOLATION: <feature-slug>.TNN
Flagged files:
- <path>: [reason it is outside task scope]
```

---

## Part 3 — Review Completeness Check

_Run before committing (Step 9 of task-by-task-development.md)._

### 3.1 Acceptance Criteria Verification

For each AC ID in the task's traceability column:

| AC ID | Verified by | Evidence | Status |
| ----- | ----------- | -------- | ------ |
| `<slug>.AC1` | `<test or observable>` | `<result>` | `Satisfied / Not satisfied / Partial` |

- [ ] **[BLOCKING]** All listed AC IDs are satisfied (not merely assumed).
- [ ] **[BLOCKING]** No AC was excluded or silently deferred.

### 3.2 API Contract Verification

- [ ] **[BLOCKING if API exists]** Each `<slug>.APINN` referenced by the task
      is implemented correctly.
- [ ] **[BLOCKING if API exists]** No API behaviour was invented that is not
      in the approved spec contract.
- [ ] State `Not applicable` if no API surface for this task.

### 3.3 Test Evidence

- [ ] **[BLOCKING]** All relevant tests have been run.
- [ ] **[BLOCKING]** All relevant tests pass (GREEN).
- [ ] **[BLOCKING]** RED evidence was confirmed before this implementation.
- [ ] No test was weakened or removed to achieve GREEN.
- [ ] No expected outcome was changed merely to pass a test.

### 3.4 Security and Constitution

- [ ] **[BLOCKING]** No security constraint from the spec or constitution is violated.
- [ ] **[BLOCKING]** No secrets or PII are present in any changed file, comment,
      test data, or configuration.
- [ ] Security-relevant spec behaviour is addressed in the implementation or
      explicitly marked inapplicable for this task.

### 3.5 Architecture Compliance

- [ ] **[BLOCKING]** No architectural change is present that was not in the approved plan.
- [ ] **[BLOCKING]** No unregistered dependency, external API, data store,
      service, or integration was introduced.
- [ ] **[BLOCKING]** No ADR is required that was not already resolved.

### 3.6 Scope Compliance

- [ ] **[BLOCKING]** No deferred scope item from the plan is implemented.
- [ ] **[BLOCKING]** No work from another task is included.
- [ ] No unrelated refactoring is mixed in.
- [ ] No opportunistic cleanup beyond task scope.

### 3.7 Review Occurrence

- [ ] **[BLOCKING]** Human or governed review of the generated changeset
      occurred before this commit check.
- [ ] Review record exists for this task (task ID, AC IDs, files, outcome).
- [ ] Review outcome was `Correct` or `Partially correct with local correction complete`.

**Part 3 result:**

```
REVIEW COMPLETE: <feature-slug>.TNN — ready to commit
```

or:

```
REVIEW INCOMPLETE: <feature-slug>.TNN
Blocking conditions:
- [condition 1]
- [condition 2]
```

---

## Part 4 — Commit Compliance Check

_Run when preparing or inspecting a commit for a task._

### 4.1 Commit Message Format

- [ ] **[BLOCKING]** Commit message includes the task ID in traceable form:
      `Implements <feature-slug>.TNN` or equivalent.
- [ ] **[BLOCKING]** Commit message does NOT contain AI attribution language:
      - `Generated by Claude` → BLOCKED
      - `Generated by Codex` → BLOCKED
      - `AI-assisted` → BLOCKED
      - `Written by AI` → BLOCKED
      - `Co-authored by AI` or similar → BLOCKED
- [ ] Commit message does not use vague language (`misc changes`, `fixes`, `updates`).

### 4.2 Commit Scope

- [ ] **[BLOCKING]** Commit does not bundle multiple task IDs' work without
      explicit justification from the approved task model.
- [ ] **[WARNING]** If multiple commits exist for one task: all commits
      should trace to the same task ID.

### 4.3 Task State Consistency

- [ ] Task state in `.ai-context/tasks/<feature-slug>.tasks.md` is updated to
      `In Review` after commit (awaiting Gate 2).
- [ ] **[BLOCKING]** Task state is NOT `Merged` unless Gate 2 has completed
      and the change has been merged to the main/trunk branch.

**Part 4 result:**

```
COMMIT COMPLIANT: <feature-slug>.TNN — <commit hash or pending>
```

or:

```
COMMIT NON-COMPLIANT: <feature-slug>.TNN
Blocking conditions:
- [condition 1]
```

---

## Part 5 — Status and Traceability Check

_Run when updating status at any point._

- [ ] Feature slug appears in `.ai-context/status.md` Active Specs table.
- [ ] **[BLOCKING if missing]** Active feature in development must appear in status board.
- [ ] Task ID is visible or traceable in the status or notes field.
- [ ] **[BLOCKING]** Feature spec status reflects the actual current lifecycle state.
  - `In Development` while tasks are actively being implemented.
  - `In QA` when all tasks are committed and Gate 2 is pending.
  - Do not advance status beyond what the evidence supports.
- [ ] Status was updated on the same day as the lifecycle transition.

**Part 5 result:**

```
STATUS CURRENT: <feature-slug>
```

or:

```
STATUS STALE: <feature-slug>
Required updates:
- [item 1]
```

---

## Part 6 — Significantly Wrong Generation Check

_Run when deciding whether to re-prompt or escalate._

### 6.1 Failure source classification

If a generation is wrong, answer these diagnostic questions in order:

1. **Is the task ID clear?** If the prompt had no task ID → Source A (prompt defect).
2. **Does the task boundary make sense?** If the task is too broad or internally inconsistent → Source B (task defect).
3. **Does the plan resolve the approach?** If the plan is silent on how to do this → Source C (plan defect).
4. **Do the ACs define the behaviour?** If the AC is vague, conflicting, or missing → Source D (spec defect).
5. **Is an architectural decision unresolved?** If the agent cannot determine architecture from approved artefacts → Source E (ADR/arch defect).
6. **Is the context complete?** If required source files were not loaded → Source F (context defect).
7. **Was the code logic simply wrong?** With correct artefacts and context → Source G (implementation defect).

### 6.2 Corrective action by source

| Source | Action |
| ------ | ------ |
| A — Prompt defect | Fix the prompt; re-prompt for this task |
| B — Task defect | **STOP**; fix the task in tasks.md; return to Part 1 |
| C — Plan defect | **STOP**; fix the plan; re-run architecture check if needed |
| D — Spec defect | **STOP**; fix the spec through Gate 1 process |
| E — ADR/arch defect | **STOP**; resolve through adr-check / architecture-check |
| F — Context defect | Fix context scope; re-prompt for this task |
| G — Implementation defect | Correct locally; re-run Part 3 |

### 6.3 Detecting excessive re-prompting

**WARNING** when:
- The same generation error has occurred more than two times for the same task
  after local corrections were applied.

**BLOCKING** when:
- After two or more attempts at local correction the same semantic
  misunderstanding persists. This is no longer a local implementation defect
  (source G). Stop, classify as B, C, D, E, or F, and escalate.

Do not create an endless conversational repair loop. The goal is:

```
clean regeneration from a corrected source
```

not:

```
iterative patching until the output is "good enough"
```

**Part 6 result:**

```
GENERATION FAILURE CLASSIFIED:
Source:          [A / B / C / D / E / F / G]
Failure:         [description]
Required action: [fix prompt / fix task / fix plan / fix spec / resolve ADR / fix context / local correction]
Next step:       [return to step X of task-by-task-development.md]
```

---

## Findings Format

For each issue found, use this structured format:

```
ID:                  DD-<AREA>-NNN
Severity:            BLOCKING | WARNING | NON-BLOCKING
Area:                <area code>
Observation:         <what was found>
Why it matters:      <what governance principle is violated>
Evidence:            <specific content, ID, or absent item>
Required action:     <what must be done before proceeding>
```

Area codes:

| Code | Area |
| ---- | ---- |
| RDY | Task readiness |
| UPD | Upstream chain (spec/plan/Gate 1) |
| TRC | Traceability (AC/API IDs) |
| SCP | Changeset scope |
| RVW | Review completeness |
| TST | Test-first evidence |
| CMT | Commit compliance |
| AI  | AI attribution |
| STS | Status accuracy |
| WRG | Significantly wrong generation |
| ESC | Escalation/upstream routing |

---

## Scenario Quick Reference

| Scenario | Expected check result |
| -------- | --------------------- |
| Prompt has no task ID | Part 1 — RDY — BLOCKING |
| Two tasks in one prompt | Part 1 — SCP — BLOCKING |
| Spec not approved | Part 1 — UPD — BLOCKING |
| Gate 1 not completed | Part 1 — UPD — BLOCKING |
| RED not confirmed | Part 1 — TST — BLOCKING |
| File outside task scope | Part 2 — SCP — BLOCKING |
| Deferred scope in diff | Part 2 — SCP — BLOCKING |
| AC not satisfied | Part 3 — TRC — BLOCKING |
| Test not run | Part 3 — TST — BLOCKING |
| Commit before review | Part 4 — RVW — BLOCKING |
| "Generated by Claude" in commit | Part 4 — AI — BLOCKING |
| Task marked Merged without Gate 2 | Part 4/5 — STS — BLOCKING |
| Same error after two corrections | Part 6 — WRG — WARNING then BLOCKING |
