# Feature Tasks Template

> Template only. Copy to `.ai-context/tasks/<feature-slug>.tasks.md` only after the linked plan is approved. Replace all placeholders; do not treat template text as executable work.

## Task Set ID

`<feature-slug>`

## Task State Convention

Each task uses one explicit state: `Not Started` → `In Progress` → `In Review` → `Merged`.

## Derived From

- **Approved plan:** `.ai-context/plans/<feature-slug>.plan.md`
- **Approved spec:** `.ai-context/specs/<feature-slug>.spec.md`

## Execution Rules

- Each task is bounded, independently understandable, reviewable, and verifiable.
- Each task maps to relevant acceptance criteria and/or API requirements.
- One task is executed per focused prompt; validate and review before moving to the next task.
- Do not add architecture, requirements, or unrelated refactoring not defined upstream.

## Ordered Tasks

| Task ID | State | Ordered work | Scope boundary | Traceability | Verification |
| --- | --- | --- | --- | --- | --- |
| `<feature-slug>.T01` | `Not Started` | `<independently verifiable work>` | `<in-scope modules and explicit exclusions>` | `<AC/API IDs>` | `<test or observable verification>` |
| `<feature-slug>.T02` | `Not Started` | `<independently verifiable work>` | `<in-scope modules and explicit exclusions>` | `<AC/API IDs>` | `<test or observable verification>` |

## Dependencies and Sequencing Notes

- `<dependency or none>`

## Blockers / Open Decisions

- `<Not specified | Conflicting | Requires decision | TBD>`

## Boundary

This task set defines **what exact ordered sequence is executed**. It is not a substitute for the spec or plan. Implementation may execute only approved tasks and must use test-first practice.
