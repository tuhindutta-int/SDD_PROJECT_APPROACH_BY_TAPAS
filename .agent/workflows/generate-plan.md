# Generate Plan Workflow

## Authority

Create a reviewable implementation plan for an approved feature specification.
Do not implement while planning. Do not bypass the constitution and architecture
checks before task generation.

Governing rule: `.agent/rules/int-sdd-architecture.md`
Plan template: `.ai-context/plans/_template.plan.md`
Architecture check: `.agent/workflows/architecture-check.md`
ADR check: `.agent/workflows/adr-check.md`
Lifecycle: `.ai-context/lifecycle.md`

---

## Required Precondition

The feature spec must be `Approved` (Gate 1 completed with a named human
non-author reviewer). A plan must NOT be drafted for a spec that has not
passed Gate 1.

---

## Inputs Required

- Explicit feature slug: `<feature-slug>`
- `.ai-context/specs/<feature-slug>.spec.md` — Status: `Approved`
- `.ai-context/constitution.md` — current version
- `.ai-context/architecture.md` — relevant sections
- `.ai-context/decisions/` — relevant existing ADRs
- Related specs and plans referenced in the spec's Context section

---

## Mandatory Plan Derivation Chain

```
Approved Spec
    ↓
Plan (this workflow)
    ↓
Constitution Check  ← Run: .agent/workflows/architecture-check.md Part 1
    ↓
Architecture Check  ← Run: .agent/workflows/architecture-check.md Parts 2–8
    ↓
ADR Assessment     ← Run: .agent/workflows/adr-check.md where applicable
    ↓
Task Generation    ← Only after ARCHITECTURE CHECK — PASS
```

**Do not generate tasks as part of this workflow.**
Task generation begins only from an accepted plan that has passed the
constitution and architecture checks.

---

## Plan Authoring Procedure

1. Read the approved spec, applicable constitution constraints, referenced
   architecture sections, and relevant ADRs.

2. Identify and explicitly document in the plan:
   - Integration points (name every integration touched; state `Not applicable` if none)
   - Data model impact (name every change; state `Not applicable` if none)
   - Deferred items (name every deferral; state `None — no items deferred` if none)
   - Constitution compliance items (test-first, security, NFRs, versioning)
   - Significant architectural decisions (reference existing ADR or flag for ADR assessment)

3. Trace each plan element to explicit acceptance criteria or an approved decision.
   Do not infer requirements, architecture, APIs, data models, technologies, or
   integrations that are unspecified.

4. State sequencing and verification points in increments that can be reviewed
   before task generation.

5. Mark missing information as **Not specified**, **Conflicting**,
   **Requires decision**, or **TBD** — report it rather than guessing.

6. Do not introduce new product behaviour. The plan is HOW; the spec is WHAT.
   Any plan element that invents product behaviour must be removed. Return to
   the spec if product behaviour is unresolved.

7. Write the plan to `.ai-context/plans/<feature-slug>.plan.md` only when
   authorized by the lifecycle state.

---

## After Plan Is Drafted

1. Run `.agent/workflows/architecture-check.md` against the plan.
2. For any significant architectural decision identified: run
   `.agent/workflows/adr-check.md`.
3. Create required ADRs before task generation.
4. Update `.ai-context/architecture.md` where required.
5. Only after `ARCHITECTURE CHECK — PASS`: proceed to task generation.

---

## Completion Check

Before declaring the plan ready for architecture check:

- [ ] Plan derives from the approved spec only — no invented product behaviour.
- [ ] Integration points explicitly named or declared `Not applicable`.
- [ ] Data model impact explicitly named or declared `Not applicable`.
- [ ] Deferred items explicitly listed or declared `None — no items deferred`.
- [ ] Constitution compliance section complete.
- [ ] Significant architectural decisions identified and ADR need assessed.
- [ ] No secrets, PII, or confidential data in the plan.
- [ ] No out-of-scope work introduced.
- [ ] Plan does not generate tasks or implementation — plan authoring is its own stage.
