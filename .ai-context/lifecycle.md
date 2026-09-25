# INT SDD Delivery Lifecycle

## Authority and Default Path

This is the authoritative repository reference for lifecycle stages, state transitions, required evidence, and feedback loops. The default path is:

```text
Discovery → Constitution → Spec → Gate 1 → Plan → Architecture Check → Tasks
→ Test-first (RED) → Guided Implementation (GREEN) → Gate 2 → Merge → Release
→ Production Support → Feedback into Spec / BRD
```

Do not reorder, skip, or collapse stages because a change appears simple. Later-stage artefacts must not justify earlier-stage decisions. A later discovery returns work to the artefact that owns the decision.

## Stage Responsibilities

| Stage | Purpose and required artefact/evidence | Next valid stage | Blocking conditions | Responsible human role |
| --- | --- | --- | --- | --- |
| Discovery / Consulting | Capture validated business problem, affected parties, outcome, constraints, known systems/data ownership, decisions, and open items in `.ai-context/BRD.md` | Spec | Business need or scope unresolved; solution conflicts with business need | Business owner / stakeholder |
| Constitution | Apply project-wide law from `.ai-context/constitution.md`; identify exception needs | Spec | Feature would contradict constitution without governance decision | Project governance owner |
| Spec | Define intent, correctness, ACs, API contract if applicable, scope, and non-functional constraints in the feature spec | Gate 1 | BRD link missing, ambiguity, untestable ACs, undefined scope | Spec author |
| Gate 1 | Peer review spec for ambiguity, testability, scope, dependencies, API, constitution, architecture/security implications | Plan | No named non-author reviewer; outcome not recorded as `Approved` or `Changes Requested` | Named human peer reviewer; security/architecture reviewer where affected |
| Plan | Derive technical approach from approved spec and constitution | Architecture Check | Plan adds product behaviour or lacks necessary technical context | Plan author / technical owner |
| Architecture Check | Compare plan with constitution, architecture, and relevant ADRs; record required ADRs | Tasks | Architecture/constitution violation, unapproved datastore/service/integration/API/security decision | Architect / technical authority where applicable |
| Tasks | Create ordered, bounded, reviewable tasks with AC/API traceability | Test-first | Plan not accepted; task is broad, untraceable, or introduces scope | Task author / engineering owner |
| Test-first (RED) | Create reviewed spec-derived test cases and executable tests; run and record relevant failure | Guided Implementation | Tests missing/unreviewed, no AC mapping, RED not confirmed, test/environment defect | Engineer with test reviewer where defined |
| Guided Implementation (GREEN) | Execute one approved task within scoped context; run tests to GREEN | Gate 2 | Scope change, ambiguity, conflict, security issue, or test-first evidence absent | Engineer |
| Gate 2 | Review actual diff, ACs, task scope, RED/GREEN evidence, security, architecture/ADR/docs, and unrelated changes | Merge | Human review incomplete or findings unresolved | Named human code reviewer |
| Merge | Integrate human-approved work with stable SDD traceability | Release | Gate 2 missing; merge criteria incomplete | Human merge authority |
| Release | Verify release readiness, spec status, release documentation, scope, tests/security, and current status | Production Support | Release criteria or documentation incomplete | Release authority |
| Production Support | Capture observation, incident, impact, and root cause; preserve traceability | Feedback | Finding not classified or artefact destination unknown | Support / product / engineering owner |
| Feedback | Update BRD, existing spec, architecture, ADR, or new cycle according to decision ownership | Discovery or Spec | Intended behaviour changed only in code; decision not formalized | Responsible business or technical owner |

## Feature Specification States

```text
Draft → In Peer Review → Changes Requested → In Peer Review → Approved
→ Plan Drafted → Plan Reviewed → Tasks Generated → In Development → In QA
→ Ready for Release → Released (vX.Y.Z) → Deprecated / Superseded
```

All transitions are explicit. A file existing, code existing, tests passing, a PR existing, an AI review, or a chat statement does not prove completion. Evidence in the governing artefacts and status board does.

| Current state | Required inputs and evidence | Next state | Blockers |
| --- | --- | --- | --- |
| Draft | Linked validated BRD, complete feature contract | In Peer Review | Missing/ambiguous intent, ACs, API, scope, or constraints |
| In Peer Review | Named non-author reviewer; recorded Gate 1 outcome | Approved or Changes Requested | Informal or self approval; unresolved review concern |
| Changes Requested | Spec revisions address recorded feedback | In Peer Review | Feedback not resolved |
| Approved | Formal Gate 1 evidence; constitution review | Plan Drafted | Missing human approval or required sign-off |
| Plan Drafted | Technical plan derived from approved spec | Plan Reviewed | Plan changes product behaviour or lacks approach |
| Plan Reviewed | Architecture check against constitution, architecture, ADRs; plan accepted | Tasks Generated | Violation/conflict/required ADR unresolved |
| Tasks Generated | Bounded tasks trace to plan and AC/API IDs; required test-case work identified | In Development | Task not ready or test-first prerequisites absent |
| In Development | One task, scoped context, reviewed tests with confirmed RED, implementation and GREEN evidence | In QA | No RED, test failure, scope/decision conflict |
| In QA | Actual diff, AC verification, security/docs/architecture review evidence | Ready for Release | Gate 2 incomplete or findings unresolved |
| Ready for Release | Release scope, status, documentation, test/security criteria complete | Released (vX.Y.Z) | Release authority or readiness evidence missing |
| Released (vX.Y.Z) | Released version recorded | Deprecated / Superseded, when applicable | None; future change follows feedback cycle |

Task states are: `Not Started → In Progress → In Review → Merged`. Track them in the feature task artefact and `.ai-context/status.md` as applicable.

## Gates and No-Skip Rules

- Gate 1 is a formal human peer review before technical execution; only `Approved` authorizes planning onward.
- When security or architecture constitution areas are affected, Gate 1 requires the applicable named security or architecture sign-off in addition to peer-review evidence.
- Architecture violations discovered during plan review are Gate 1/plan-stage blockers, not late code-review comments.
- Test-first requires confirmed RED before implementation.
- Gate 2 is a human review of the actual diff before merge.
- Release is distinct from merge and requires explicit readiness evidence.
- Prompts, code, tests, tasks, and production patches must not redefine upstream intent. Return to the owning BRD, spec, plan, constitution, architecture document, or ADR instead.

## Hotfix Lifecycle

A genuine production emergency may use `HOTFIX-<incident-slug>` with a lightweight spec, correct behaviour/ACs, root-cause analysis, patch, Gate 2 review, and post-hoc formalization where applicable. Gate 1 may be deferred only for a genuine emergency; Gate 2 is never skipped. A hotfix is not a permanent SDD bypass.

## Status Board Currency

`.ai-context/status.md` is the repository-level SDD state view. Add active specifications with Spec ID, title, status, owner, last-updated date, and notes/blockers; update it on the same day as each lifecycle state transition. It reports current flight status and does not replace detailed feature task artefacts or an external delivery tracker.

## Trivial Changes

Only changes with no behavioural or architectural effect—such as formatting or a confirmed configuration bump—may be classified as trivial. Do not use this category to bypass SDD for meaningful behavioural or architecture changes. If classification is unclear, stop for a human decision.

## Production Feedback Loop

```text
Production → Observation / Incident / New Requirement → Root Cause / Business Impact
→ BRD or Existing Spec → Revised or New Requirement → New SDD Cycle
```

Production findings may require a spec clarification, BRD entry, AC refinement, new spec, defect fix, architecture update, ADR, incident record, or process improvement. New intended behaviour must be captured in its governing artefact; never only in code.
