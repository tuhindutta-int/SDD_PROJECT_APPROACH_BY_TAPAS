# Task-by-Task Development Workflow

## Authority

This is the authoritative implementation execution workflow for INT SDD Line 10.

Governing rule: `.agent/rules/int-sdd-development.md`
Non-negotiables: `.agent/rules/int-sdd-non-negotiables.md`
Naming identity: `.agent/rules/int-sdd-naming.md`
Lifecycle: `.ai-context/lifecycle.md`
Pre-implementation check: `.agent/workflows/pre-implementation-compliance.md`
Review workflow: `.agent/workflows/code-review.md`

---

## Purpose

Operationalize the development discipline rule as a step-by-step execution
guide. An engineer or agent following this workflow will proceed task by task,
one prompt at a time, with explicit generate → review → commit checkpoints and
a defined stop-and-escalate path for ambiguity or significantly wrong generation.

---

## Inputs Required

- Feature slug: `<feature-slug>`
- Approved spec: `.ai-context/specs/<feature-slug>.spec.md`
- Approved plan: `.ai-context/plans/<feature-slug>.plan.md`
- Approved tasks: `.ai-context/tasks/<feature-slug>.tasks.md`
- Current task ID: `<feature-slug>.TNN`
- Linked AC IDs from the spec
- Linked API IDs from the spec (where applicable)
- Status board: `.ai-context/status.md`

---

## PRECONDITIONS — Verify Before ANY Task Execution

Run all preconditions before executing the first task (and verify again
if resuming after a break).

### P1 — Full SDD Chain in Place

- [ ] **[BLOCKING]** Spec exists: `.ai-context/specs/<feature-slug>.spec.md`
- [ ] **[BLOCKING]** Spec status is `Approved`
- [ ] **[BLOCKING]** Gate 1 has a named human non-author reviewer and recorded approval
- [ ] **[BLOCKING]** Plan exists: `.ai-context/plans/<feature-slug>.plan.md`
- [ ] **[BLOCKING]** Plan has passed the Architecture Check
- [ ] **[BLOCKING]** Tasks exist: `.ai-context/tasks/<feature-slug>.tasks.md`
- [ ] **[BLOCKING]** Feature branch exists: `feature/<feature-slug>`

Run `.agent/workflows/pre-implementation-compliance.md` and confirm it passes
before proceeding.

### P2 — Test-First Prerequisites

- [ ] **[BLOCKING]** Test cases exist in `.ai-context/test_cases/<feature-slug>.test_cases.md`
- [ ] **[BLOCKING]** Executable tests are created and reviewed for the tasks being implemented
- [ ] **[BLOCKING]** RED evidence has been confirmed for the relevant test cases
  - Record: test command, output, and reason for failure before implementation

### P3 — Status Board Updated

- [ ] Feature slug appears in `.ai-context/status.md` Active Specs table
- [ ] Status reflects current lifecycle state (`In Development`)

---

## MAIN LOOP — Repeat for Each Task

Execute this loop once per task, in task sequence order.

---

### STEP 1 — SELECT ONE TASK

- [ ] Identify the next `Not Started` task in the ordered task list.
- [ ] Record the task ID: `<feature-slug>.TNN`
- [ ] Confirm sequencing: any prior task that must complete first is `Merged`.

If a sequencing dependency is not met: **STOP. Do not skip tasks.**

---

### STEP 2 — VERIFY TASK READINESS

Run Part 1 of `.agent/workflows/development-discipline-check.md`.

Confirm:

- [ ] **[BLOCKING]** Task ID `<feature-slug>.TNN` exists in the tasks file
- [ ] **[BLOCKING]** Task state is `Not Started` or `In Progress` (resuming)
- [ ] **[BLOCKING]** Task description is specific enough to implement without guessing
- [ ] **[BLOCKING]** Task's AC traceability column references existing, non-retired AC IDs
- [ ] **[BLOCKING]** Task's API traceability references existing, non-retired API IDs (if applicable)
- [ ] **[BLOCKING]** No unresolved architectural decision is required before this task
- [ ] **[BLOCKING]** RED evidence for this task's test cases is confirmed
- [ ] **[BLOCKING]** Task scope boundary is explicitly stated

If any BLOCKING item fails:

**DO NOT PROCEED.**
Record the blocking condition.
Route to the appropriate upstream artefact (task, plan, spec, or ADR).
Do not attempt to implement around the gap.

**OUTPUT: `TASK READY` or `TASK NOT READY — <blocking condition>`**

---

### STEP 3 — LOAD SCOPED CONTEXT

Load ONLY the context required for this specific task.

Required:

| Context item | Notes |
| ------------ | ----- |
| Approved spec | Full or relevant sections only |
| Approved plan | Relevant sections for this task |
| Tasks file | Current task entry only |
| Linked AC IDs | From spec |
| Linked API IDs | From spec (if applicable) |
| Relevant ADRs | Only ADRs referenced by this task/plan |
| Relevant architecture sections | Only sections relevant to this task |
| Source files in scope | Only files named in the task's scope boundary |
| Test files in scope | Only tests for this task's ACs |
| Stack standards | Where applicable |

Do NOT load:
- The entire repository
- Unrelated feature artefacts
- Entire conversation history as specification context
- Files not in the task's scope boundary

---

### STEP 4 — UPDATE TASK STATUS

Update the task state in `.ai-context/tasks/<feature-slug>.tasks.md`:

```
<feature-slug>.TNN | Not Started → In Progress
```

Update `.ai-context/status.md` if the spec-level status requires updating.

---

### STEP 5 — PROMPT BY TASK IDENTITY

Construct a compliant implementation prompt using stable IDs.

Required prompt elements:

```
Implement <feature-slug>.TNN.

Verify against:
- <feature-slug>.ACN   [list all linked AC IDs]
- <feature-slug>.APINN [if applicable]

Context (load only):
- .ai-context/specs/<feature-slug>.spec.md
- .ai-context/plans/<feature-slug>.plan.md
- .ai-context/tasks/<feature-slug>.tasks.md
- [specific source files for this task]
- [specific test files for this task]

Do not modify:
- Work belonging to prior tasks
- Work belonging to future tasks
- Deferred scope
- Unrelated modules

Expected verification:
- [test ID] progresses to GREEN
- No unexpected files modified
```

**Non-compliant prompts are REJECTED:**
- No task ID in the prompt → REJECTED
- Multiple task IDs in the prompt → REJECTED
- "Implement the entire feature" → REJECTED
- "Continue" or "carry on" without explicit task ID → REJECTED

---

### STEP 6 — GENERATE

Execute the implementation against the selected task.

The agent generates a changeset scoped to `<feature-slug>.TNN`.

During generation:
- Agent must stay within the task scope boundary
- Agent must not implement work from other tasks
- Agent must not perform unrelated refactoring or cleanup
- Agent must not make architectural decisions not in the approved plan

After generation: do not proceed to commit. The next step is inspection.

---

### STEP 7 — INSPECT DIFF

Before review, inspect the raw generated changeset.

Run Part 2 of `.agent/workflows/development-discipline-check.md`.

Perform a scope triage:

- [ ] List every file changed.
- [ ] Confirm each file is in the task's declared scope boundary.
- [ ] Flag any file outside the scope boundary as a potential scope violation.
- [ ] Confirm no deferred scope items appear in the diff.
- [ ] Confirm no changes to files belonging to other tasks.

If the diff contains unexpected files:

**STOP. Classify the issue (see Step 9 — Classify).**
Do not proceed to review an out-of-scope diff.

**Scope-clean indicator:** Every changed file is explainable as necessary
for `<feature-slug>.TNN` and its approved contract.

---

### STEP 8 — REVIEW

Run `.agent/workflows/code-review.md` scoped to this task.

Review checklist (task-scoped):

- [ ] **[BLOCKING]** Each AC in the task's traceability column is satisfied
- [ ] **[BLOCKING]** API contract (if applicable) is not violated
- [ ] **[BLOCKING]** Test-first tests have progressed from RED → GREEN
- [ ] **[BLOCKING]** No security constraint violated; no secrets/PII introduced
- [ ] **[BLOCKING]** No architectural change not present in the approved plan
- [ ] **[BLOCKING]** No deferred scope implemented
- [ ] Unexpected files — none or documented as task-necessary
- [ ] No unrelated behaviour introduced
- [ ] No unrelated changes (refactors, cleanup, formatting churn)
- [ ] Repository standards satisfied (no AI attribution, Line 9 ID consistency)
- [ ] Naming and ID consistency verified

Record the review outcome using the task review record format:

```
Task ID:             <feature-slug>.TNN
AC IDs verified:     <feature-slug>.AC1, .AC2, …
API IDs verified:    <feature-slug>.API01, … (or N/A)
Files changed:       [list]
Tests run:           [test IDs and outcomes]
Review outcome:      Correct / Partially correct / Significantly wrong / Blocked
Issues found:        [list or None]
Corrective action:   [list or None]
Commit reference:    [pending / <commit hash> when committed]
Reviewer/owner:      <name or role>
```

---

### STEP 9 — CLASSIFY REVIEW OUTCOME

Based on review, classify:

#### CORRECT → Proceed to Step 10 (Commit)

Change satisfies the task and its approved contract. No issues found or
minor issues corrected within this review cycle.

#### PARTIALLY CORRECT → Correct Locally, Review Again

Local implementation error (source G). The change is mostly right;
an obvious, task-scoped implementation mistake is present and correctable
without modifying the specification or plan.

- Correct the specific local error within task scope.
- Do not expand scope during correction.
- Return to Step 8 (Review) for the corrected change.

#### SIGNIFICANTLY WRONG → STOP (see §7 of int-sdd-development.md)

Generation fundamentally misunderstands the requirement or violates the
approved contract.

Classify the failure source:

| Source | Diagnosis | Correct action |
| ------ | --------- | -------------- |
| A — Prompt defect | Correct artefacts; wrong prompt | Fix the prompt; return to Step 5 |
| B — Task defect | Task is ambiguous or incorrectly bounded | Fix the task in tasks.md; return to Step 2 |
| C — Plan defect | Technical approach is incomplete | Fix the plan; run architecture check if required |
| D — Spec defect | AC conflicts, missing behaviour | Fix the spec through Gate 1 process |
| E — ADR/arch defect | Required decision unresolved | Resolve through adr-check / architecture-check |
| F — Context defect | Wrong/missing context | Fix context scope; return to Step 3 |
| G — Implementation defect | Logic error in otherwise clear task | Correct locally; return to Step 8 |

When source is B, C, D, or E: **STOP ALL IMPLEMENTATION.**

```
STOP re-prompting
    ↓
Record the failure and its classified source
    ↓
Fix the authoritative source (task / plan / spec / ADR)
    ↓
Follow change-propagation rules for stale downstream artefacts
    ↓
Return to Step 2 (Task Readiness) with corrected artefacts
    ↓
Regenerate from clean, corrected source
```

Do not patch the code to compensate for an incorrect specification.
Do not re-prompt repeatedly hoping for convergence.

#### BLOCKED BY UPSTREAM AMBIGUITY → STOP

Missing requirement, conflicting artefacts, unresolved ADR, or undefinable
expected output.

```
Record the ambiguity:
    AMBIGUITY:         <what is unclear>
    SOURCE:            <which artefact is deficient>
    IMPACT:            <what cannot be implemented without resolution>
    REQUIRED DECISION: <what must be resolved before proceeding>
    ↓
Route to the appropriate upstream artefact
    ↓
Do not implement workarounds
    ↓
Resume only after the authoritative source is corrected
```

---

### STEP 10 — COMMIT

Commit the reviewed, verified changeset.

Pre-commit checks:

- [ ] Review outcome was `Correct` (not `Significantly wrong` or `Blocked`)
- [ ] All test-first tests pass (GREEN)
- [ ] No unexpected files in the commit
- [ ] No AI attribution language in the commit message
- [ ] Commit message references the task ID for traceability

Commit message pattern:

```
Implements <feature-slug>.TNN

<Optional one-line description of what changed>
```

This is traceability, not AI attribution.

Commit this task's changes as a distinct, task-scoped commit.
Do not bundle multiple tasks into one commit without explicit justification
from the approved task model.

---

### STEP 11 — UPDATE STATUS AND TRACEABILITY

After commit:

- [ ] Update task state in `.ai-context/tasks/<feature-slug>.tasks.md`:
      `<feature-slug>.TNN | In Review` (awaiting Gate 2 merge)
- [ ] Update commit reference in the task review record.
- [ ] Update `.ai-context/status.md` if feature-level state changed.
- [ ] Record progress in `.ai-context/prompt_history.md` where the task
      represents a significant implementation event.

Note: Task state moves to `Merged` only after Gate 2 human review and merge.
`In Review` signals that the commit is pending Gate 2, not that it is merged.

---

### STEP 12 — NEXT TASK

After Step 11, decide:

- **Tasks remain:** Return to Step 1. Select the next `Not Started` task.
- **All tasks complete:** Do not proceed to merge autonomously.
  Confirm Gate 2 is pending. Update the feature spec status to `In QA` when
  appropriate. Do not mark the spec `Released` before Gate 2 and Release gates.

Do not skip to a future task because its files are already open.
Do not implement a future task that "seems like it goes naturally from here."
Task sequencing is defined in the approved tasks artefact.

---

## FAILED CHANGE HANDLING

When a change is rejected after review:

### Local implementation error (source G)

- Task remains `In Progress`.
- Correct the local error within task scope.
- Re-run Step 8 (Review) for the corrected change.
- Do not expand scope during correction.

### Significantly wrong or upstream ambiguity (sources A–F)

- Task returns to `Not Started` (or remains `In Progress` pending upstream fix).
- Record the root cause and failure source.
- Fix the authoritative source (task / plan / spec / ADR).
- Apply change-propagation rules for any stale downstream artefacts.
- Return to Step 2 with corrected artefacts.
- Regenerate cleanly from the corrected source.

---

## Anti-Pattern Reference

| If this occurs | It is non-compliant because | Required action |
| -------------- | --------------------------- | --------------- |
| Prompt contains no task ID | No implementation identity | Rewrite prompt with task ID |
| Prompt authorizes "the feature" | No task boundary | Decompose to one task |
| Prompt authorizes T01 + T02 | Multiple tasks | Separate into two prompts |
| Agent modifies files outside task scope | Scope violation | Stop; classify; regenerate |
| Agent implements deferred scope | Unauthorized scope | Stop; classify as B or C; escalate |
| Generation repeatedly misunderstands AC | Spec/task ambiguity | Stop re-prompting; fix upstream |
| Commit made before review | Bypasses quality gate | Reset to Step 8 before committing |
| Commit message says "Generated by Claude" | AI attribution | Remove; use task ID traceability |
| Task marked `Merged` because generation succeeded | Generation ≠ merge | Gate 2 must complete first |

---

## Scenario Validation Reference

| Scenario | Expected outcome |
| -------- | ---------------- |
| A: Prompt says "Implement the entire feature" | BLOCKED/NON-COMPLIANT — Step 5 |
| B: Prompt explicitly references T03 and only T03 | COMPLIANT — Step 5 |
| C: Agent modifies T03 plus unrelated T05 files | SCOPE VIOLATION — Step 7 |
| D: T03 generation has obvious local coding error | LOCAL CORRECTION — Step 9 (Partially correct) |
| E: T03 generation repeatedly misunderstands behaviour | STOP RE-PROMPTING — Step 9 (Significantly wrong, D or B source) |
| F: Task depends on an unstated requirement | BLOCKED — Step 2 (Not Ready) |
| G: Developer reviews before committing | COMPLIANT — Step 8 before Step 10 |
| H: Agent generates T01+T02+T03, presents aggregate diff | NON-COMPLIANT — Step 6 prohibition |
| I: Commit message contains "Generated by Claude" | BLOCKED — Step 10 AI attribution check |
| J: Commit references "Implements `<slug>.T03`" | COMPLIANT traceability — Step 10 |
| K: Task implemented without approved spec/Gate 1 | BLOCKED — Precondition P1 |
| L: Corrected spec makes existing task stale | Follow change-propagation; return to Step 2 |
| M: Different model used for different task | ALLOWED — model choice does not modify contract |
