# Feature Tasks Template

> Template only. Copy to `.ai-context/tasks/<feature-slug>.tasks.md` only after
> the linked plan is approved and the Architecture Check has passed. Replace all
> placeholders; do not treat template text as executable work.
>
> Governing rule: `.agent/rules/int-sdd-development.md`
> Execution workflow: `.agent/workflows/task-by-task-development.md`
> Discipline check: `.agent/workflows/development-discipline-check.md`

---

## Task Set ID

`<feature-slug>`

_Must match the feature slug in `.ai-context/specs/<feature-slug>.spec.md`
and all other feature artefacts. See `.agent/rules/int-sdd-naming.md` §8._

---

## Task State Convention

Each task uses one explicit state:

```
Not Started → In Progress → In Review → Merged
```

- `Not Started` — approved; awaiting implementation.
- `In Progress` — active implementation session under way.
- `In Review` — committed; Gate 2 pending.
- `Merged` — Gate 2 completed; merged to main/trunk.

`Merged` requires Gate 2 human review and branch merge.
`In Review` does NOT mean merged.
Generated code is not reviewed code. Reviewed code is not merged code.

---

## Derived From

- **Approved spec:** `.ai-context/specs/<feature-slug>.spec.md`
- **Approved plan:** `.ai-context/plans/<feature-slug>.plan.md`
- **Architecture check:** Passed (required before tasks are generated)
- **Constitution:** `.ai-context/constitution.md`

---

## Execution Rules

Governed by `.agent/rules/int-sdd-development.md`. Key rules:

- One task is executed per focused prompt. One prompt, one task.
- Validate task readiness before executing: run
  `.agent/workflows/development-discipline-check.md` Part 1.
- Generate, then inspect, then review, then commit. Do not commit unreviewed code.
- Verify against the linked AC IDs and API IDs — not vague prose expectations.
- Do not implement deferred scope, work from other tasks, or unrelated cleanup.
- If generation is significantly wrong: stop and diagnose the failure source
  (task, plan, spec, or ADR defect) before re-prompting.
- Do not add architecture, requirements, or unrelated refactoring not defined upstream.
- Use stable task IDs in prompts: `Implement <feature-slug>.TNN.`

---

## Ordered Tasks

| Task ID | State | Ordered work | Scope boundary | Traceability (AC/API IDs) | Verification | Context required |
| --- | --- | --- | --- | --- | --- | --- |
| `<feature-slug>.T01` | `Not Started` | `<independently verifiable work>` | `<in-scope modules; explicit exclusions>` | `<slug>.AC1, .AC2` | `<test ID or observable>` | `<source files; ADR refs>` |
| `<feature-slug>.T02` | `Not Started` | `<independently verifiable work>` | `<in-scope modules; explicit exclusions>` | `<slug>.AC3` | `<test ID or observable>` | `<source files; ADR refs>` |

_Add a column for `Explicit non-goals` where task boundary ambiguity is likely._

---

## Dependencies and Sequencing Notes

- `<dependency or none>`
- Tasks with mandatory ordering: T01 must be `Merged` before T02 can begin — state explicitly when true.

---

## Test-First Prerequisites

| Task ID | Test case IDs | RED confirmed | RED evidence location |
| --- | --- | --- | --- |
| `<feature-slug>.T01` | `<slug>.UT01, .UT02` | `Not yet / Yes` | `<command / file reference>` |
| `<feature-slug>.T02` | `<slug>.UT03` | `Not yet / Yes` | `<command / file reference>` |

_RED must be confirmed before implementation begins for each task._
_Do not mark RED confirmed without actually running and recording the failing test._

---

## Implementation Prompt Template

When implementing a task, use stable IDs in the prompt:

```
Implement <feature-slug>.TNN.

Verify against:
- <feature-slug>.ACN
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
- [test ID] passes GREEN
```

---

## Blockers / Open Decisions

- `<Not specified | Conflicting | Requires decision | TBD>`

---

## Boundary

This task set defines **what exact ordered sequence is executed**. It is not a
substitute for the spec or plan.

- Implementation may execute only approved tasks, one at a time.
- Test-first practice is mandatory: confirmed RED before implementation.
- Review occurs before commit for every task.
- Gate 2 human review is required before merge.
- Do not implement deferred items or scope from other tasks.

Governing rule for implementation execution: `.agent/rules/int-sdd-development.md`
