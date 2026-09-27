# ADR-NNNN — <Decision Title>

> Template only. Copy to `.ai-context/decisions/ADR-NNNN-<decision-slug>.md`
> when an architectural decision is approved. Replace all placeholders.
> Do not use this template file as an actual ADR.
>
> ADR IDs are project-global sequential numbers. Check the existing ADR list
> in `.ai-context/decisions/` to determine the next available number.
> Use the ADR assessment workflow at `.agent/workflows/adr-check.md`
> to determine whether an ADR is required.

---

## Status

`Proposed` | `Accepted` | `Deprecated` | `Superseded by ADR-NNNN`

## Date

`<YYYY-MM-DD>`

## Feature / Context

`<feature-slug or project-wide>`

---

## Context

Describe the situation that requires this decision.

- What is the problem, opportunity, or constraint?
- What forces or constraints shape this decision?
- What is the scope (feature-specific or project-wide)?
- What existing architecture or ADR is relevant?

Do not describe the decision here — describe why a decision is needed.

---

## Decision

State the decision clearly and unambiguously.

One or two sentences maximum. The reader should be able to understand what
was decided without reading the Alternatives section.

This decision may NOT contradict `.ai-context/constitution.md` without an
explicit governance exception recorded here.

---

## Alternatives Considered

| Alternative | Brief description | Why not chosen |
| ----------- | ----------------- | -------------- |
| `<alternative>` | `<description>` | `<reason>` |

Do not omit this section. If no alternatives were considered, state why
alternatives were not evaluated.

---

## Consequences

### Positive

- `<positive consequence>`

### Negative / Trade-offs

- `<trade-off or limitation>`

### Neutral

- `<neutral consequence>`

---

## Downstream Impact

List artefacts affected by this decision:

| Artefact | Impact | Action required |
| -------- | ------ | --------------- |
| `.ai-context/architecture.md` | `<section>` | Update to reflect this decision |
| `.ai-context/specs/<slug>.spec.md` | `<area>` | Review for consistency |
| `.ai-context/plans/<slug>.plan.md` | `<area>` | Review for consistency |

---

## Related ADRs

- `ADR-NNNN — <title>` (superseded / amended / related)
- `None`

---

## References

- Governing spec: `.ai-context/specs/<feature-slug>.spec.md`
- Plan: `.ai-context/plans/<feature-slug>.plan.md`
- Architecture section: `.ai-context/architecture.md#<section>`
- ADR assessment: `.agent/workflows/adr-check.md`

---

## Gate Record

| Gate | Reviewer | Date | Outcome |
| ---- | -------- | ---- | ------- |
| ADR review | `<name>` | `<YYYY-MM-DD>` | `Accepted / Changes Requested` |
