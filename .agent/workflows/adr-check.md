# ADR Assessment Workflow

## Authority

This workflow determines whether an architectural decision encountered in a
plan or architecture check requires an Architectural Decision Record (ADR).

Governing rule: `.agent/rules/int-sdd-architecture.md` §5–7
ADR store: `.ai-context/decisions/`
Architecture: `.ai-context/architecture.md`

---

## Purpose

Not every implementation choice requires an ADR. This workflow ensures:

- Significant decisions are captured (not left in chat, plan prose, or commits).
- Trivial choices are not over-documented.
- Existing ADRs are not duplicated.
- Architecture is not invented opportunistically.

---

## Input Required

- Description of the decision or choice encountered.
- The plan or spec that surfaced the decision.
- `.ai-context/decisions/` — existing ADR list.
- `.ai-context/architecture.md` — current architecture record.

---

## Step 1 — Is There an Architectural Decision?

**Not all implementation choices are architectural decisions.**

Architectural decisions concern:

- System structure (new service, new component, new boundary)
- Data management (new datastore, schema strategy, migration approach)
- Integration approach (how services/systems communicate)
- Technology selection (framework, library, protocol — when it has lasting impact)
- Cross-cutting concerns (auth, security model, observability strategy)
- Significant constraints (performance-sensitive, security-sensitive choices)

Implementation choices that are **NOT** architectural decisions:

- Variable naming
- File organization within a module
- Minor refactoring within existing patterns
- Formatting or linting choices
- Swapping one equally-suitable helper library for another with no wider impact

**Decision:** Is this an architectural decision?

```
Yes → proceed to Step 2
No  → no ADR required; document the choice in the plan if useful
```

---

## Step 2 — Significance Test

**INT Significance Rule:**

> A decision is significant when reversing it later would cost more than
> one day of rework.

Apply the test:

| Question | Answer |
| -------- | ------ |
| What would it cost (effort-days) to reverse this decision after implementation? | |
| Does the decision affect multiple features or components? | |
| Does the decision establish a pattern that future features will follow? | |
| Does the decision constrain future architectural options? | |
| Would changing the decision after implementation require migration, data changes, or protocol changes? | |

**Decision:** Is the decision significant?

```
Yes → ADR is required; proceed to Step 3
No  → ADR not required; inline the rationale in the plan
Unclear → treat as significant; flag for human decision before proceeding
```

---

## Step 3 — Does an ADR Already Exist?

Search `.ai-context/decisions/` for an existing ADR that covers this decision.

Check existing ADRs:

| ADR ID | Decision slug | Status | Covers this decision? |
| ------ | ------------- | ------ | --------------------- |
| | | | |

**Decision:** Does a usable ADR already exist?

```
Yes — existing ADR covers the decision completely:
    → Reference the existing ADR in the plan.
    → No new ADR required.

Yes — existing ADR partially covers the decision:
    → Determine whether the existing ADR should be amended or superseded.
    → If superseded: create a new ADR that references the prior one.
    → Do not silently diverge from an existing ADR without recording the change.

No — no ADR exists:
    → ADR is required; proceed to Step 4.
```

---

## Step 4 — Is the Decision Already in `architecture.md`?

Check whether `.ai-context/architecture.md` already documents this decision
as current architecture.

**Decision:**

```
Yes — architecture.md records the decision and approach:
    → If the plan follows the documented approach: no new ADR required.
    → If the plan diverges from the documented approach: a new ADR IS required.

No — architecture.md does not record this:
    → ADR is required.
    → architecture.md update will be required after the ADR is approved.
```

---

## Step 5 — Downstream Impact Assessment

Before creating or requiring an ADR, identify what is affected:

| Downstream artefact | Affected? | Action required |
| ------------------- | --------- | --------------- |
| `architecture.md` | | Update after ADR approval |
| Other specs | | Identify for review |
| Other plans | | Identify for review |
| Existing tasks | | Identify for review |
| Existing tests | | Identify for review |
| Existing implementation | | Identify for review |

Do not automatically rewrite downstream artefacts. Identify scope first,
then require human decision before updating.

---

## Step 6 — ADR Required Output

If an ADR is required, produce the following assessment:

```markdown
# ADR Assessment

## Decision assessed
<description of the decision>

## Significance
Significant: Yes / No / Unclear
Reason: <why reversal would cost more than one day, or why it is not significant>

## Existing ADR
Existing ADR found: Yes / No
If yes: <ADR-NNNN reference>
Disposition: <references / amends / supersedes / no new ADR required>

## Recommendation
ADR REQUIRED | ADR NOT REQUIRED

## If ADR required:
- Proposed ADR ID: ADR-NNNN (next sequential project-global number)
- Proposed slug: <decision-slug>
- Proposed file: .ai-context/decisions/ADR-NNNN-<decision-slug>.md
- Downstream artefacts affected: <list>
- Required before: <Plan approval / Task generation / Implementation>
```

---

## ADR Template

When an ADR is required, use this template at the path
`.ai-context/decisions/ADR-NNNN-<decision-slug>.md`:

```markdown
# ADR-NNNN — <Decision Title>

## Status

`Proposed` | `Accepted` | `Deprecated` | `Superseded by ADR-NNNN`

## Date

<YYYY-MM-DD>

## Feature / Context

<feature-slug or project-wide>

## Context

Describe the situation that requires a decision. What is the problem or
opportunity? What constraints apply? What alternatives exist?

## Decision

State the decision clearly and unambiguously.

## Alternatives Considered

| Alternative | Why not chosen |
| ----------- | -------------- |
| | |

## Consequences

### Positive
- <positive consequences>

### Negative / Trade-offs
- <trade-offs and limitations>

### Neutral
- <neutral consequences>

## Related ADRs

- <ADR-NNNN references or None>

## References

- `<spec or plan that prompted this decision>`
- `<architecture section updated>`
```

---

## Critical Reminders

1. **Do not create an ADR for every implementation choice** — only significant ones.
2. **Do not allow a significant decision to remain only in plan prose or chat.**
3. **ADR IDs are project-global — do not scope them by feature.**
4. **A required ADR that does not exist is a blocking finding in the architecture check.**
5. **After an ADR is accepted, update `architecture.md` to reflect the current state.**
