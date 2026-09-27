# Feature Plan Template

> Template only. Copy to `.ai-context/plans/<feature-slug>.plan.md` only
> after the linked spec has Gate 1 approval. Replace all placeholders.
> Do not treat template text as a decision or approved approach.
>
> This plan must pass the Architecture Check
> (`.agent/workflows/architecture-check.md`) before task generation proceeds.

---

## Plan ID

`<feature-slug>`

## Status

`Plan Drafted` | `Plan Reviewed` | `Architecture Check Passed`

## Derived From

- **Approved spec:** `.ai-context/specs/<feature-slug>.spec.md`
- **Constitution:** `.ai-context/constitution.md`
- **Architecture:** `.ai-context/architecture.md#<relevant-section or Not applicable>`
- **Relevant ADRs:** `<ADR-NNNN references or None>`

---

## Technical Approach

Describe how the approved feature contract will be technically achieved.

Map the approach to acceptance criteria IDs (`<feature-slug>.ACN`) without
redefining or narrowing intended behaviour.

Do not introduce product requirements. The plan is HOW — the spec is WHAT.

---

## Modules, Components, and Services

| Area | Change type | Responsibility | Related AC/API ID |
| --- | --- | --- | --- |
| `<module/path or Requires decision>` | `new / change / none` | `<technical responsibility>` | `<AC/API IDs>` |

---

## Integration Points

_MANDATORY — state `Not applicable` if this feature touches no integration._

_Name every integration affected: external API, internal service boundary,
authentication system, data store, event system, third-party component,
platform capability, or existing module boundary._

For each integration:

| Integration | What changes | Relevant contract | Impact | Dependency |
| ----------- | ------------ | ----------------- | ------ | ---------- |
| `<name or Not applicable>` | | | | |

---

## Data Model Impact

_MANDATORY — state `Not applicable` if no data model change is required._

_Name every change: schema, entity, relationship, migration, persistence,
transient/session state._

| Change | Description | Migration required? | Impact |
| ------ | ----------- | ------------------- | ------ |
| `<change or Not applicable>` | | | |

---

## Sequencing and Verification

Ordered technical steps with verification points. Each step should be
reviewable before the next begins.

1. `<ordered step — verification: <AC/API ID or observable>>`

---

## Constitution Compliance

_Check each area. Silence is not compliance — address or explicitly mark
`Not applicable`._

- [ ] **Testing discipline:** plan is consistent with spec → AC → test-first →
      implementation; no test-bypassing approach introduced.
- [ ] **Security:** security-relevant spec behaviour is addressed in the approach,
      or marked `Requires decision`.
- [ ] **Architectural constraints:** no unspecified technology, infrastructure, or
      integration introduced.
- [ ] **Non-functional baselines:** applicable NFRs are addressed or referenced.
- [ ] **Versioning:** consistent with constitution versioning rules.
- [ ] **Repository rules:** no secrets/PII, no AI attribution.

Constitution compliance notes: `<any constraint addressed, exception, or gap>`

---

## Significant Architectural Decisions

_MANDATORY — assess each architectural impact using
`.agent/workflows/adr-check.md`._

| Decision | Significant? | Existing ADR | ADR required? |
| -------- | ------------ | ------------ | ------------- |
| `<decision or None>` | `Yes / No / Unclear` | `ADR-NNNN or None` | `Yes / No` |

Architecture.md update required: `Yes — <sections> / No`

---

## Deferred Implementation Items

_MANDATORY — state `None — no items deferred` if nothing is deferred._

_Name every deferral explicitly. Silently omitting deferred scope is not
permitted. Gate 2 will verify implementation did not scope-creep into
deferred items._

| Deferred item | Reason for deferral | Future spec or ADR |
| ------------- | ------------------- | ------------------ |
| `<item or None — no items deferred>` | | |

---

## Open Items and Decisions Required

| Item | Status | Required action |
| ---- | ------ | --------------- |
| `<item>` | `Not specified / Conflicting / Requires decision / TBD` | `<action>` |

---

## Boundary

This plan defines **HOW** to meet the approved spec. It must not:

- Introduce product requirements or redefine acceptance criteria.
- Silently choose product-level behaviour not defined in the spec.
- Invent API behaviour, data-model decisions, or integration semantics
  not in the approved spec.

Any element that invents product behaviour must be removed. If product
behaviour is unresolved, return to the spec and update it through the
Gate 1 process.

Downstream artefact: `.ai-context/tasks/<feature-slug>.tasks.md`
Created only after `ARCHITECTURE CHECK — PASS`.
