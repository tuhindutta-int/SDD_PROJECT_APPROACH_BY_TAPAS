# Feature Plan Template

> Template only. Copy to `.ai-context/plans/<feature-slug>.plan.md` only after the linked spec has Gate 1 approval. Replace all placeholders; do not treat template text as a decision.

## Plan ID

`<feature-slug>`

## Derived From

- **Approved spec:** `.ai-context/specs/<feature-slug>.spec.md`
- **Constitution:** `.ai-context/constitution.md`
- **Relevant architecture:** `.ai-context/architecture.md#<relevant-section>`
- **Relevant ADRs:** `<relative-paths-or-none>`

## Technical Approach

Describe how the approved feature contract will be technically achieved. Map the approach to spec acceptance criteria without changing intended behaviour.

## Modules, Components, and Services

| Area | Change type | Responsibility | Related AC/API |
| --- | --- | --- | --- |
| `<module/path or Requires decision>` | `<new/change/none>` | `<technical responsibility>` | `<AC/API IDs>` |

## Integration Points and Data Model

- **Integrations:** `<specified integrations or Not applicable>`
- **Data model impact:** `<specified impact or Not applicable>`
- **Dependencies:** `<approved dependency or Requires decision>`

## Sequencing Considerations

1. `<technical ordering constraint>`

## Technical Constraints and Constitution Compliance

- `<constraint and authoritative source>`
- [ ] Testing discipline reviewed
- [ ] Security posture reviewed
- [ ] Architectural constraints reviewed
- [ ] Non-functional baselines reviewed

## Significant Architectural Decisions

- `<existing ADR reference, proposed ADR, or None>`

## Deferred Implementation Items

- `<explicitly deferred item and reason>`

## Open Items and Decisions Required

- `<Not specified | Conflicting | Requires decision | TBD>`

## Boundary

This plan defines **how** to meet the approved spec. It must not introduce product requirements, redefine acceptance criteria, or silently choose unspecified product behaviour. Its downstream artefact is `.ai-context/tasks/<feature-slug>.tasks.md`.
