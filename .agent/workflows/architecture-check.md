# Architecture Check Workflow

## Authority

This workflow enforces the Architecture Check stage of the INT SDD lifecycle.
It runs after a plan is drafted and before tasks are generated.

Governing rule: `.agent/rules/int-sdd-architecture.md`
ADR assessment: `.agent/workflows/adr-check.md`
Constitution: `.ai-context/constitution.md`
Architecture: `.ai-context/architecture.md`
Lifecycle: `.ai-context/lifecycle.md` — stage: Architecture Check

**This workflow must produce either:**

```
ARCHITECTURE CHECK — PASS
```

or

```
ARCHITECTURE CHECK — BLOCKED
```

Task generation may proceed only after a PASS outcome from a responsible
human technical reviewer. An automated PASS from this checklist is not human
architectural sign-off; it is a pre-review analysis.

---

## Inputs Required

Assemble before running this workflow:

- [ ] `.ai-context/specs/<feature-slug>.spec.md` — Status: `Approved`
- [ ] `.ai-context/plans/<feature-slug>.plan.md` — Status: `Plan Drafted` or later
- [ ] `.ai-context/constitution.md` — current version
- [ ] `.ai-context/architecture.md` — current version
- [ ] `.ai-context/decisions/` — any relevant existing ADRs
- [ ] Any related specs or plans referenced in the Context or Dependencies section

If the plan is missing or the spec is not `Approved`: **STOP. Do not run.**

---

## Part 1 — Constitution Compliance Check

This is the mandatory plan → constitution gate.

**SILENCE IS NOT COMPLIANCE.**

If the constitution imposes a meaningful constraint and the plan is silent on
how that constraint is satisfied or intentionally not applicable: **FLAG IT.**

### Testing Discipline

- [ ] **[BLOCKING]** Plan does not bypass the spec → AC → test-first → implementation
      sequence.
- [ ] **[BLOCKING]** Plan does not invent acceptance criteria not in the approved spec.
- [ ] Plan identifies which ACs drive the implementation approach.

### Security

- [ ] **[BLOCKING if applicable]** Security-relevant behaviour identified in the spec is
      explicitly addressed in the plan or flagged as `Requires decision`.
- [ ] No secrets, credentials, PII, or confidential data appear in the plan.

### Architectural Constraints

- [ ] **[BLOCKING]** Plan does not introduce unspecified technology, infrastructure,
      or integration that is not in the approved spec or approved architecture.
- [ ] Plan is consistent with `.ai-context/architecture.md` where architecture
      is defined.
- [ ] Plan does not silently change a service boundary or integration contract.

### Non-Functional Constraints

- [ ] NFRs that apply to the feature are referenced in the plan with their
      authoritative source (constitution or approved decision).
- [ ] No invented performance, availability, or compliance targets.
- [ ] Unresolved NFRs are marked `Requires decision`.

### Versioning

- [ ] Plan does not introduce a versioning approach that contradicts the
      constitution's versioning rules.

### Repository Rules

- [ ] No AI attribution appears in the plan.
- [ ] Plan does not reference secrets or PII.

**Constitution check result:** `Pass` | `Blocked — findings listed in §8`

---

## Part 2 — Integration Points

- [ ] **[BLOCKING]** Plan explicitly identifies all integration points affected by this
      feature, or states `Not applicable`.
- [ ] For each integration identified: what is touched, what changes, what contract
      is relevant, and what impact is expected.
- [ ] Plan does not silently assume an integration that is not in the spec or
      current architecture.
- [ ] Any new integration not currently in `architecture.md` is flagged for
      architecture.md update.

**Integration check result:** `Pass` | `Blocked — findings listed in §8`

---

## Part 3 — Data Model

- [ ] **[BLOCKING]** Plan explicitly identifies data model impact, or states
      `Not applicable`.
- [ ] Schema, entity, relationship, migration, persistence, and session-state
      changes are named.
- [ ] Data model choices are derived from the approved spec — not invented by
      the plan.
- [ ] Any new data model element not currently in `architecture.md` is flagged
      for architecture.md update.

**Data model check result:** `Pass` | `Blocked — findings listed in §8`

---

## Part 4 — Deferred Items

- [ ] **[BLOCKING]** Plan explicitly lists deferred items, or states
      `None — no items deferred`.
- [ ] Deferred items are not silently omitted.
- [ ] Deferred items do not represent work that should be in the current feature
      scope (which would indicate scope incompleteness in the spec).
- [ ] Deferred items do not represent hidden dependencies.

**Deferred items check result:** `Pass` | `Blocked — findings listed in §8`

---

## Part 5 — Architectural Impact Assessment

For each of the following: state `Yes`, `No`, or `Unclear — requires decision`.

| Impact area | Present? | Notes |
| ----------- | -------- | ----- |
| New datastore or persistence layer | | |
| New service or component boundary | | |
| Changed service or component boundary | | |
| New external integration | | |
| Changed integration contract | | |
| New major internal module/component | | |
| Changed data ownership model | | |
| Performance-sensitive decision | | |
| Security-sensitive decision | | |
| Authentication or authorization model change | | |
| Infrastructure or deployment change | | |

Any `Yes` or `Unclear` items that are not already covered by an approved ADR
and not already reflected in `architecture.md` must be assessed using
`.agent/workflows/adr-check.md`.

**Architectural impact check result:** `Pass` | `Blocked — ADR assessment required`

---

## Part 6 — ADR Requirement Assessment

For each architectural impact identified in Part 5:

Run `.agent/workflows/adr-check.md` to determine whether an ADR is required.

Summary:

| Decision | Significant? | Existing ADR? | ADR required? |
| -------- | ------------ | ------------- | ------------- |
| `<decision>` | `Yes / No / Unclear` | `ADR-NNNN or None` | `Yes / No` |

**If ADR is required and does not exist:** this is a BLOCKING finding.
Task generation must not proceed until the required ADR is created and recorded.

**ADR assessment result:** `Pass` | `Blocked — ADR required`

---

## Part 7 — `architecture.md` Currency Check

Review `.ai-context/architecture.md` against the plan.

- [ ] Current `architecture.md` accurately reflects the existing system (not stale).
- [ ] Plan does not introduce changes that `architecture.md` does not reflect.
- [ ] If the plan introduces architectural changes: identify exactly which sections
      of `architecture.md` require updating.

If `architecture.md` update is required:

- Identify: which sections, what changes.
- Record as a required action in the check output (§8).
- `architecture.md` update must be committed as part of this feature's SDD change.

**architecture.md check result:** `Pass` | `Update required (sections listed in §8)`

---

## Part 8 — No Spec Bypass Verification

- [ ] **[BLOCKING]** Plan derives exclusively from the approved spec.
- [ ] **[BLOCKING]** Plan does not introduce product requirements not in the spec.
- [ ] **[BLOCKING]** Plan does not redefine or narrow acceptance criteria.
- [ ] **[BLOCKING]** Plan does not invent API behaviour not in the spec's API contract.
- [ ] Plan does not silently expand scope beyond the spec's feature boundary.

If any blocking item fails: **do not generate tasks**. Return to the spec or
plan for correction through the appropriate governance process.

**Spec bypass check result:** `Pass` | `Blocked — findings listed below`

---

## Architecture Check Output

Produce the following report:

```markdown
# Architecture Check — <feature-slug>

## Check Metadata

| Field            | Value |
| ---------------- | ----- |
| Feature slug     | <feature-slug> |
| Spec status      | Approved |
| Plan file        | .ai-context/plans/<feature-slug>.plan.md |
| Architecture.md  | reviewed |
| ADRs reviewed    | <list or None> |
| Reviewer         | <name or AI-assisted pre-check> |
| Check date       | <YYYY-MM-DD> |

## Result

**ARCHITECTURE CHECK — PASS**
or
**ARCHITECTURE CHECK — BLOCKED**

## Blocking Findings

| ID | Area | Observation | Required action |
|----|------|-------------|-----------------|
| AC-<AREA>-NNN | | | |

Finding area codes:
CON = Constitution | INT = Integration | DAT = Data Model
DEF = Deferred | ARC = Architecture Impact | ADR = ADR Required
ARC-DOC = architecture.md | SPB = Spec Bypass

## Required Actions

| Action | Owner | Priority |
| ------ | ----- | -------- |
| | | |

## architecture.md Updates Required

| Section | Change required |
| ------- | --------------- |
| | |

## ADRs Required

| Decision | ADR slug | Owner |
| -------- | -------- | ----- |
| | | |

## Summary by Check Area

| Area | Result | Notes |
| ---- | ------ | ----- |
| Constitution compliance | Pass / Blocked | |
| Integration points | Pass / Blocked | |
| Data model | Pass / Blocked | |
| Deferred items | Pass / Blocked | |
| Architectural impact | Pass / Blocked | |
| ADR assessment | Pass / Blocked | |
| architecture.md currency | Pass / Update required | |
| Spec bypass | Pass / Blocked | |

## Authorization to Proceed

ARCHITECTURE CHECK — PASS means automated checks are complete.
Task generation requires the responsible technical reviewer to confirm
that architectural decisions are sound. Record that confirmation here.

Reviewer: <name>
Date: <YYYY-MM-DD>
```

---

## Critical Reminders

1. **Do not generate tasks before this check produces PASS.**
2. **Do not invent architecture during this check — only evaluate what the plan states.**
3. **Silence in the plan on a meaningful constraint is a FLAG, not a pass.**
4. **An architecture.md update is a required deliverable, not optional housekeeping.**
5. **AI running this workflow is assistance — human architectural sign-off is separate.**
