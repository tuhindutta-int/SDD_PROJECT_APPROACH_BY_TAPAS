# INT SDD Development Discipline

## Authority

This is the authoritative repository rule for INT SDD Line 10 Development
Discipline. It governs how implementation work is executed once the full
SDD artefact chain is approved.

Related artefacts:
- Development execution workflow: `.agent/workflows/task-by-task-development.md`
- Development discipline check: `.agent/workflows/development-discipline-check.md`
- Task readiness precondition: `.agent/workflows/pre-implementation-compliance.md`
- Task-scoped review: `.agent/workflows/code-review.md`
- Non-negotiable enforcement: `.agent/rules/int-sdd-non-negotiables.md`
- Naming and identity: `.agent/rules/int-sdd-naming.md`
- Architecture standards: `.agent/rules/int-sdd-architecture.md`
- Lifecycle: `.ai-context/lifecycle.md`
- Constitution: `.ai-context/constitution.md`

---

## 1. The Core Contract

**IMPLEMENTATION MAY PROCEED ONLY AGAINST AN APPROVED TASK.**

The following units define the development discipline:

| Unit | What it is |
| ---- | ---------- |
| **Implementation unit** | One task |
| **Prompting unit** | One prompt |
| **Review unit** | One generated changeset scoped to one task |
| **Commit unit** | One reviewed, task-scoped logical delivery unit |

The standard operating cycle is:

```
SELECT one approved task
    ↓
VERIFY task readiness (pre-implementation-compliance.md)
    ↓
LOAD scoped context only
    ↓
PROMPT by task identity
    ↓
GENERATE change
    ↓
INSPECT diff
    ↓
VERIFY against task ID, AC IDs, tests, scope
    ↓
REVIEW (code-review.md)
    ↓
COMMIT (after review)
    ↓
UPDATE status / traceability
    ↓
PROCEED to next task
```

Do not allow the agent to silently proceed to the next task after generation.

---

## 2. One Task, One Prompt

**ONE TASK. ONE PROMPT.**

A prompt that authorizes implementation must contain exactly one task ID.

### 2.1 What a compliant prompt contains

A compliant implementation prompt may contain:

- One task ID (`<feature-slug>.TNN`)
- Its linked acceptance-criteria IDs (`<feature-slug>.ACN`)
- Relevant API IDs (`<feature-slug>.APINN`)
- Relevant ADR references (`ADR-NNNN`) where applicable
- Explicitly named context files/sections required for the task
- Task constraints and verification expectations
- Explicit non-goals for this task

### 2.2 What a prompt must NOT authorize

A single implementation prompt must NOT implicitly or explicitly authorize:

- The entire feature
- All remaining tasks
- Unrelated refactoring
- Unrelated cleanup
- Speculative improvements
- Changes to deferred scope
- Changes to future task IDs
- Changes outside the named scope boundary
- Unrelated architectural changes

### 2.3 Compliant prompt pattern

```
Implement <feature-slug>.T03.

Verify against:
- <feature-slug>.AC3
- <feature-slug>.API02

Context:
- .ai-context/specs/<feature-slug>.spec.md
- .ai-context/plans/<feature-slug>.plan.md
- .ai-context/tasks/<feature-slug>.tasks.md
- [specific source files required by this task only]

Do not modify:
- Work belonging to <feature-slug>.T01
- Work belonging to <feature-slug>.T02
- Deferred scope listed in the plan
- Unrelated modules

Expected verification:
- <feature-slug>.UT03 passes (GREEN)
- No unexpected files changed
```

This is a governance pattern. Do not create a fake real feature from this.

### 2.4 Non-compliant prompt patterns

The following are explicitly non-compliant and must be rejected:

| Non-compliant pattern | Why it fails |
| --------------------- | ------------ |
| `Implement the entire feature.` | No task identity; authorizes unbounded scope |
| `Implement T01 and T02.` | Two tasks; violates one-task-one-prompt |
| `Implement the rate-limiting logic.` | No stable task ID; relies on prose |
| `Continue from where you left off.` | No task identity; relies on session memory |
| `Implement T03 and clean up the existing code.` | Mixed scope; cleanup is not the task |
| `Implement T03 and also fix the thing in T05.` | Multiple tasks in one prompt |

---

## 3. Task Readiness

A task must be explicitly ready before an implementation agent is invoked.

### 3.1 Readiness checklist

A task is READY FOR IMPLEMENTATION when ALL of the following are confirmed:

| Check | Requirement |
| ----- | ----------- |
| Task exists | `<slug>.TNN` is present in the approved tasks file |
| Task has a valid ID | Format follows Line 9 naming conventions |
| Task has a stable state | State is `Not Started` (or `In Progress` if resuming) |
| Parent spec is approved | Gate 1 completed; named human non-author reviewer recorded |
| Task is derived from the approved plan | Plan has passed architecture check |
| Task maps to AC IDs | `<slug>.ACN` references exist and are valid |
| Acceptance criteria are testable | No vague ACs; each is observable |
| Dependencies are resolved | Prior tasks that must complete first are `Merged` or not sequencing-blocked |
| Architectural decisions are approved | No unresolved ADR required before this task |
| Task contradicts neither plan nor spec | No discovered conflict requiring upstream resolution |
| Test-first prerequisites are met | Test cases exist; executable tests are created; RED is confirmed |
| Context is scoped | Required files and sections are identified |

### 3.2 Not-ready conditions — explicitly blocking

The following conditions block task execution:

- Task ID does not exist in the tasks file
- Parent spec status is not `Approved`
- Gate 1 was not completed or has no named reviewer
- Task acceptance criteria reference IDs that do not exist in the spec
- An architectural decision required for this task has not been resolved
- A dependency task is not yet `Merged` and ordering is mandatory
- Task description is so broad or ambiguous that it could be implemented two different ways
- Test-first RED has not been confirmed for the task's test cases
- "The agent will figure it out" — this does not qualify as readiness

Do not proceed when readiness is in doubt. Report the blocking condition and route upstream.

---

## 4. Small, Verifiable Increments

**GENERATE → REVIEW → COMMIT**

This is the standard implementation loop. It must not be compressed or batched.

### 4.1 The required sequence per task

```
T01: Generate → Review → Commit
T02: Generate → Review → Commit
T03: Generate → Review → Commit
...
TNN: Generate → Review → Commit
```

Do not generate multiple tasks then present one aggregate diff.

### 4.2 What each step requires

| Step | What must happen |
| ---- | ---------------- |
| **Generate** | Agent produces a change scoped to the single task |
| **Inspect** | Human or agent inspects the diff before declaring it reviewable |
| **Review** | Change verified against task ID, AC IDs, API contract, tests, scope, security, architecture, Line 9 IDs |
| **Commit** | Verified, reviewed logical unit committed with task-ID traceability |

Generated output that has not been inspected must NOT be committed.
Committed output that has not been reviewed must NOT be merged (Gate 2 remains the broader human review before merge).

### 4.3 Prohibited compression patterns

| Prohibited pattern | Why it is prohibited |
| ------------------ | -------------------- |
| Generate T01+T02+T03 in one prompt | Combines tasks; violates one-task-one-prompt |
| Generate T01, silently continue to T02 | Agent self-authorizes additional scope |
| Generate T01–T05, present aggregate diff | Aggregate diff hides task-level errors; violates review unit |
| Commit without review | Bypasses quality gate |
| Mark task `Merged` because generation succeeded | Generation ≠ review ≠ merge |

---

## 5. Task Boundary and Changeset Discipline

### 5.1 The changeset test

A generated changeset is acceptable when the entire diff is explainable as
necessary to satisfy the single selected task and its approved contract.

The test question is:

```
CAN THE ENTIRE DIFF BE TRACED TO THIS SINGLE TASK AND ITS APPROVED CONTRACT?
```

If not: STOP AND REVIEW SCOPE.

### 5.2 Detect and flag in a changeset

The following patterns in a generated diff are red flags requiring scope review:

- Files not named in the task's scope boundary
- Unrelated refactors mixed with task-required changes
- Opportunistic cleanup or formatting churn not required by the task
- Dependency changes not explicitly required by the task or plan
- Architecture changes not present in the approved plan
- Changes to files belonging to other tasks
- Changes to deferred scope items
- Speculative abstractions not required by the approved contract

A task may legitimately touch multiple files. The test is traceability,
not the count of files changed.

### 5.3 Scope expansion detection

If the diff contains work belonging to another task, stop the implementation
session. Do not silently accept out-of-scope changes. Determine whether:

- The task boundary was incorrectly defined (fix the task)
- The agent exceeded its authorized scope (regenerate with tighter constraints)
- A task dependency was not visible (resolve the dependency first)

---

## 6. Review Before Commit

**COMMIT AFTER REVIEW. NOT BEFORE.**

### 6.1 Review is not optional

Generated code is not reviewed code.
Reviewed code is not merged code.

A commit represents a reviewed logical unit. It does not represent an
unreviewed generation checkpoint.

### 6.2 What the task-scoped review must verify

The review question is not "does the whole feature look okay?" It is:

```
Does this generated change correctly implement <slug>.TNN
within the approved specification, plan, architecture,
and acceptance criteria?
```

The review must check:

1. Task ID — is the correct task being implemented?
2. Acceptance criteria — is each AC in the task's traceability column satisfied?
3. API contract — does the change conform to `<slug>.APINN` where applicable?
4. Tests — are test-first tests present, have they gone RED → GREEN?
5. Security — no security constraint violated; no secrets/PII introduced
6. Architecture — no unauthorized architectural change; consistent with approved ADRs
7. Deferred scope — nothing from the deferred list was silently implemented
8. Unexpected files — all changed files are in the task's scope boundary
9. Unexpected behaviour — the change does not introduce behaviour not in the spec
10. Unrelated changes — no refactoring, cleanup, or other work mixed in
11. Repository standards — commit traceability, no AI attribution
12. Naming and ID consistency — no ID drift, no cross-feature references

### 6.3 Review outcome classification

| Classification | Meaning | Action |
| -------------- | ------- | ------ |
| `Correct` | Change satisfies the task and its contract | Commit |
| `Partially correct` | Change is mostly right; local error correctable | Correct locally, review again |
| `Significantly wrong` | Change fundamentally misunderstands the requirement | Stop; diagnose root cause (see §7) |
| `Blocked by upstream` | Missing requirement, conflicting artefact, or unresolved ADR | Stop; route upstream (see §7) |

### 6.4 Review record

For each task, record:

- Task ID
- AC IDs verified
- Files changed
- Tests run and outcome
- Reviewer/owner
- Issues found
- Corrective action taken
- Commit reference when committed

The task-level review record is separate from Gate 2, which is the broader
human review before merge. Line 10 governs implementation discipline
before/during commit; Gate 2 governs merge authorization.

---

## 7. Significantly Wrong Generation — The STOP Rule

**This is one of the most important rules in Line 10.**

### 7.1 What "significantly wrong" means

A generation is significantly wrong when one or more of the following is true:

| Indicator | Severity |
| --------- | -------- |
| Violates an acceptance criterion | Significantly wrong |
| Violates the API contract | Significantly wrong |
| Violates a constitution or security rule | Significantly wrong |
| Requires inventing missing requirements to proceed | Upstream blocked |
| Changes architecture without authorization from the plan | Significantly wrong |
| Implements deferred scope | Significantly wrong |
| Changes files belonging to a different feature or task | Significantly wrong |
| Produces a fundamentally different interpretation from the task | Significantly wrong |
| Repeatedly fails after bounded local correction attempts | Upstream blocked |
| Ambiguity in authoritative artefacts causes consistent misinterpretation | Upstream blocked |

What is NOT significantly wrong (local correction is appropriate):

| Indicator | Severity |
| --------- | -------- |
| Minor syntax or compilation error | Local implementation error |
| Obvious single-location typo | Local implementation error |
| Misnamed variable with clear intent | Local implementation error |
| Minor formatting deviation | Local implementation error |

The governance must distinguish:

```
LOCAL IMPLEMENTATION ERROR     → correct within this task review cycle
UPSTREAM SPECIFICATION ERROR   → STOP; fix the authoritative source
UPSTREAM PLAN/TASK ERROR       → STOP; fix the authoritative source
```

### 7.2 Failure source classification

When a generation is significantly wrong or persistently misunderstood,
classify the source before taking action:

| Failure source | Signs | Correct action |
| -------------- | ----- | -------------- |
| **A — Prompt defect** | Correct artefacts, wrong/incomplete prompt | Fix the prompt; re-generate |
| **B — Task defect** | Task is ambiguous, too broad, or incorrectly bounded | Fix the task in tasks.md |
| **C — Plan defect** | Technical approach is incomplete or internally inconsistent | Fix the plan (may trigger Gate 1/plan review) |
| **D — Specification defect** | Acceptance criteria are missing, conflicting, or untestable | Fix the spec through Gate 1 process |
| **E — Architecture/ADR defect** | Required architectural decision is absent or unresolved | Resolve through architecture-check / ADR process |
| **F — Context defect** | Wrong or missing context files loaded | Fix the context scope; re-generate |
| **G — Implementation defect** | Logic error within otherwise clear requirements | Correct locally within this task |

### 7.3 The upstream correction path

When the failure source is B, C, D, E, or F:

```
Significantly wrong generation
    ↓
STOP re-prompting
    ↓
Classify failure source (A–G)
    ↓
Fix the authoritative source:
    Task defect   → update tasks.md
    Plan defect   → update plan; re-run architecture check if required
    Spec defect   → update spec; re-enter Gate 1 process
    ADR/arch defect → resolve through adr-check / architecture-check
    ↓
Follow existing change-propagation rules for stale downstream artefacts
    ↓
Regenerate task from corrected artefacts
```

Do not patch code to compensate for an ambiguous or incorrect specification.
Do not keep prompting hoping the agent will converge on the right interpretation.

### 7.4 Bounded corrective prompting

Use corrective prompting only for local, clear, task-scoped implementation defects (source G).

When correction attempts reveal persistent semantic misunderstanding,
classify it as a non-G source and stop:

```
WRONG → diagnose → correct authoritative source → regenerate CLEANLY
```

not:

```
WRONG → re-prompt → partial fix → re-prompt → larger diff → hidden ambiguity
```

The objective is a clean regeneration from a corrected source — not a series
of conversational repairs that obscure the root problem.

---

## 8. Context Discipline During Implementation

For each task, load only the context required to execute that task.

### 8.1 Required context per task

Load where applicable:

- Approved spec: `.ai-context/specs/<slug>.spec.md`
- Approved plan: `.ai-context/plans/<slug>.plan.md`
- Current tasks: `.ai-context/tasks/<slug>.tasks.md` — current task section
- Linked AC IDs from the spec
- Linked API IDs from the spec
- Relevant architecture sections from `.ai-context/architecture.md`
- Relevant ADRs from `.ai-context/decisions/`
- Only the source files required by this specific task
- Only the test files relevant to this specific task
- Relevant stack standards (when established)

### 8.2 Context discipline prohibitions

Do NOT load:

- The entire repository
- All source files for the whole feature
- All conversation history as implicit specification
- Artefacts from other unrelated features
- Stale chat memory in place of current repository artefacts

The repository is the source of truth. The current session's chat history
is not the specification.

---

## 9. Prompt-by-Identity Integration

Integrate tightly with Line 9 naming governance.

Implementation prompts must reference stable task IDs.

Preferred:
```
Implement <feature-slug>.T03. Verify against <feature-slug>.AC3.
```

Not acceptable:
```
Implement the rate-limiting logic.
```

The stable ID is the authoritative reference. Surrounding prose may change;
the ID is stable.

When referring to cross-artefact requirements:

- Use `<slug>.ACN` for acceptance criteria
- Use `<slug>.APINN` for API contract requirements
- Use `ADR-NNNN` for architectural decisions
- Use `BRD-NNN` for business requirements

Do not repeat a full prose description of requirements already captured in
artefacts that have stable IDs. The prompt is a directive plus identity
plus scoped context plus verification requirements.

---

## 10. Test-First Compatibility

Line 10 does NOT weaken Line 4's test-first requirement.

Line 10 governs HOW implementation is executed.
Line 4 governs the test-first precondition for implementation.

The full sequence remains:

```
Approved spec → Approved plan → Tasks → Test-first (RED confirmed)
→ Guided Implementation (generate → review → commit) → Gate 2 → Merge
```

Test-first RED must be confirmed before the generate step. The generate step
produces implementation to move tests from RED to GREEN.

Do not use Line 10's generate-review-commit pattern to justify:
- generating implementation before tests are created
- committing before tests are run
- treating build success as test-first RED evidence

Where the task has corresponding tests:
- Identify them by stable ID (`<slug>.UTNN`)
- Verify they exist and have been run RED before generating implementation
- Verify they pass (GREEN) before committing

---

## 11. Commit Governance

### 11.1 Commit after review

COMMIT AFTER REVIEW. Never commit generated but unreviewed changes.

A commit represents a reviewed logical unit, not a generation checkpoint.

### 11.2 Commit traceability

Commits within a feature branch should reference the task ID for traceability,
consistent with Lines 8–9:

```
Implements <feature-slug>.T03
```

This is traceability — not AI attribution. The task ID identifies the
engineering change without attributing it to a tool.

### 11.3 AI attribution prohibition

Commits, PR descriptions, code comments, and implementation notes must NOT
contain:

- `Generated by Claude`
- `Generated by Codex`
- `AI-assisted`
- `Written by AI`
- `Co-authored by AI`

This rule is established by Line 8 and is enforced here. Task-ID traceability
is the correct mechanism.

---

## 12. Model-to-Task Matching

Match agent model capability to task complexity.

| Task type | Appropriate model capability |
| --------- | ---------------------------- |
| Simple boilerplate, formatting, mechanical transformations | Lighter/faster models are appropriate |
| Standard feature implementation with clear contracts | Standard reasoning capability |
| Architectural reasoning, complex business logic, security-sensitive tasks, difficult debugging | Stronger reasoning models preferred |

Model selection is a tooling decision within the task process. Changing the
model does not change the specification, acceptance criteria, or architectural
contract. Do not hard-code a specific model name as a governance requirement.

---

## 13. Task Status Integration

Integrate with Line 5 task lifecycle.

Task status must make visible which exact task has moved:

| Status | Meaning |
| ------ | ------- |
| `Not Started` | Task approved; awaiting implementation |
| `In Progress` | Active implementation session under way |
| `In Review` | Generation complete; review in progress |
| `Merged` | Gate 2 approved; change merged |

One status transition must not conceal multiple implementation steps.

- `In Review` requires a completed generation and initiated review.
- `Merged` requires Gate 2 completion, not merely local task review.

Status updates must use the task's stable ID. "Feature in progress" is not
sufficient — identify which task has moved.

---

## 14. Human Accountability

Every merged line remains the responsibility of a human engineer.

- AI generation does not transfer accountability.
- Passing tests do not constitute approval.
- An agent reviewing its own output does not constitute Gate 2.
- AI-generated tests that pass do not confirm specification correctness.

Gate 2 human review is required before merge. Gate 2 is the broader code
review established by existing SDD governance. Line 10's task-level review
is a pre-commit quality gate, not a substitute for Gate 2.

---

## 15. Anti-Pattern Controls

The following behaviours are explicitly prohibited under Line 10:

| Anti-pattern | Why it is prohibited |
| ------------ | -------------------- |
| **Vibe coding** | No task ID, no artefact chain, no review, no accountability |
| **Mega-prompting** ("Implement the entire feature") | No task scope; aggregate diff cannot be reviewed properly |
| **Multi-task unattended generation** | Bypasses one-task-one-prompt and review gate |
| **Aggregate diff review** | Hides task-level errors; violates task-scoped review unit |
| **Repeated re-prompting against ambiguity** | Masks upstream specification error; produces fragile code |
| **Patching code for unclear spec** | Compensates for a defect in an authoritative source |
| **Opportunistic future-task implementation** | Scope creep; violates task boundary |
| **Mixing unrelated refactoring into task delivery** | Expands review surface; contaminates diff traceability |
| **Committing before review** | Bypasses quality gate |
| **"Agent understood it" as task readiness** | Does not constitute readiness; readiness requires verifiable artefacts |
| **Successful build as specification correctness** | Build success proves compilation, not correctness |

---

## 16. Integration with Existing Governance

Line 10 complements existing governance without duplicating it:

| Existing governance | Where | Line 10 relationship |
| ------------------- | ----- | -------------------- |
| Test-first, RED evidence | Line 4 / non-negotiables.md | Preserved; RED must precede generate step |
| Pre-implementation compliance check | `pre-implementation-compliance.md` | Task readiness gate; run before generate |
| Code review | `code-review.md` | Task-scoped review; strengthened by Line 10 |
| Gate 2 | `lifecycle.md` | Broader pre-merge human review; separate from task review |
| Naming and identity | `int-sdd-naming.md` | Task IDs and AC IDs used for prompt-by-identity |
| Branch/commit standards | `int-sdd-architecture.md` | Commit traceability; AI attribution prohibition |
| Change propagation | `lifecycle.md`, `constitution.md` | When upstream artefact is corrected, propagation rules apply |
