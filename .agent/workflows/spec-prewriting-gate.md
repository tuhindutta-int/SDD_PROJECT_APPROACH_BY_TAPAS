# Spec Pre-Writing Readiness Gate

## Authority

This workflow is mandatory before drafting or revising any feature specification.
The authoritative standard is `.agent/rules/int-sdd-specification-standard.md`.
Do not open the spec template until every required question below is answered.

## Purpose

Prevent a specification from being authored before its minimum required context is
available. A specification that starts without this gate tends to contain vague
intent, invented requirements, and unresolved decisions disguised as facts.

## Required Questions

Answer each question explicitly before proceeding.

---

### Q0 — Feature Slug Assignment (FIRST STEP)

> Has a valid, unique feature slug been assigned for this feature?

This is the first action — before any spec content is drafted.

- Propose a slug following `.agent/rules/int-sdd-naming.md` §2:
  - Kebab-case, 3–5 words, human-readable, specific, verb-free.
  - Not a prohibited pattern (`feature-42`, `new-feature`, `changes`, `final`, `wip`, etc.).
- Confirm the slug is not in `.ai-context/slug-registry.md` (active or retired).
- Run Part 2–3 of `.agent/workflows/naming-identifier-check.md` to validate.
- Register the slug in `.ai-context/slug-registry.md` before proceeding.

If slug collision exists: **STOP. Assign a different slug.**
If slug format is invalid: **STOP. Fix the slug before proceeding.**

Record the confirmed slug: `<feature-slug>`

---

### Q1 — One-paragraph feature description

> Can the feature be described in one unambiguous paragraph that a zero-context
> engineer could understand?

- Answer with the draft paragraph, or
- State: **Not ready — description is ambiguous or missing.**

If two competent engineers reading the paragraph could build materially different
behaviour, the answer is **Not ready.**

---

### Q2 — BRD linkage

> Has the business requirement already been captured and validated in
> `.ai-context/BRD.md`?

- If yes: record the `BRD-NNN` identifier.
- If no: **STOP. Return to Discovery. Do not author a spec for a requirement that
  does not exist in BRD.md.** The spec is not the first place a business
  requirement appears.

---

### Q3 — Required context artefacts

> Are all required context artefacts available and at a usable lifecycle state?

Check:

- [ ] `.ai-context/constitution.md` — current and consistent with the feature.
- [ ] `.ai-context/architecture.md` — relevant sections available (or explicitly
  TBD and noted).
- [ ] Relevant ADRs under `.ai-context/decisions/` — identified or confirmed None.
- [ ] Related specs under `.ai-context/specs/` — identified or confirmed None.

If a required artefact is missing, note: **Requires decision** or **TBD**.

---

### Q4 — Related specifications identified

> Are specifications for features that this feature depends on or relates to
> identified?

- List related specs and their current status, or
- State: **None identified.**

Do not silently depend on a spec that does not exist or is not `Approved`.

---

### Q5 — Architecture constraints known

> Are the architecture constraints that apply to this feature known?

- Describe the applicable constraints (even if they are "stack TBD"), or
- State: **Not specified — architecture is TBD** and note the implication for the
  spec.

---

### Q6 — Design and system constraints known

> Are design, data, or system constraints relevant to this feature known?

- Describe constraints or reference the authoritative source, or
- State: **Not specified** and flag as a required decision before Gate 1.

---

### Q7 — API contract available (conditional)

> If this feature exposes or consumes an API surface: is the contractual API
> information available?

- If yes: confirm method, path, expected payload shape, and response shape are
  known to a degree that the spec can define them, or
- If no API surface: state **Not applicable — no API surface for this feature.**
- If the API exists but information is incomplete: state **Requires decision** and
  list what is missing. Do not draft the API contract section with invented values.

---

### Q8 — Required stakeholders and sign-offs known

> Are the required stakeholders and Gate 1 reviewer(s) identified?

- Name the spec author and the required named non-author Gate 1 reviewer, or
- State: **Not specified — requires assignment before Gate 1.**

---

### Q9 — Unresolved decisions explicitly identified

> Are all unresolved decisions explicitly identified rather than silently resolved?

- List every open decision with: item, impact on spec, and required action, or
- State: **No unresolved decisions at this time.**

Do not write an unresolved decision as a stated fact in the spec.

---

## Gate Decision

### PROCEED to spec authoring when:

All nine questions are answered and:

- Q1 has a single unambiguous paragraph.
- Q2 has a valid `BRD-NNN` reference.
- All unresolved items from Q3–Q9 are marked `TBD`, `Not specified`, or
  `Requires decision` — **not silently assumed**.

### STOP when:

- Q1 cannot be answered in one unambiguous paragraph.
- Q2 has no validated `BRD-NNN` entry.
- Any critical unknown is written as an assumption rather than flagged.

When stopping, record the blocking question(s) and return to Discovery or BRD
before re-entering this gate.

---

## Output

Record the answers to Q1–Q9 in the spec's **Open Items and Decisions Required**
section for any question that is not fully resolved.

Do not carry forward any invented requirement, assumed constraint, or guessed API
behaviour from this gate into the specification.
