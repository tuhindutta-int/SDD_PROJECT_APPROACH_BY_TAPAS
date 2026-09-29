# INT SDD Naming and Identifier Governance

## Authority

This is the authoritative repository rule for naming and identifier governance
(INT SDD Line 9). It establishes the feature slug as the canonical root identity
for a feature and defines the stable identifier hierarchy that flows from it.

Related artefacts:
- Validation workflow: `.agent/workflows/naming-identifier-check.md`
- Specification standard: `.agent/rules/int-sdd-specification-standard.md`
- Architecture / repository standards: `.agent/rules/int-sdd-architecture.md`
- Spec template: `.ai-context/specs/_template.spec.md`
- Constitution: `.ai-context/constitution.md`
- Lifecycle: `.ai-context/lifecycle.md`

---

## 1. Why the Slug Is Not Cosmetic

A feature slug is the single traceability thread for a feature across its entire
lifecycle. It is not a filename choice. It is not a label that can be changed when
a better name occurs to someone. It is not a convenience shorthand.

The slug is the root identifier from which all other artefact identities are
derived. It must remain stable from the moment a spec is created until the feature
is deprecated and archived.

If the slug is cosmetic, the following break simultaneously:

- Branch-to-spec traceability
- PR-to-spec traceability
- Task-to-AC cross-references
- Test-to-AC traceability
- Status board identifiability
- Release note traceability
- ADR cross-references into the feature
- Agent prompting by identity
- Historical audit trail

The slug is governance infrastructure, not decoration.

---

## 2. What a Feature Slug Is

A feature slug is the canonical root identifier for exactly one feature.

It is assigned at the moment the feature's specification is created and remains
stable for the feature's lifetime.

### 2.1 Format requirements

| Rule | Requirement |
| ---- | ----------- |
| Character set | Lowercase letters, digits, hyphens only (`[a-z0-9-]`) |
| Case | Kebab-case (all lowercase, words separated by single hyphens) |
| Length | Approximately 3–5 words |
| Specificity | Unique across the project lifetime, including retired slugs |
| Orientation | Names the domain capability or thing — not the implementation action |
| Stability | Assigned once; not changed while downstream artefacts exist |

### 2.2 Slug quality criteria

A valid slug is:

- **Human-readable** — a competent engineer unfamiliar with the feature can
  understand roughly what the feature is about from the slug alone.
- **Specific** — it cannot be confused with any other feature in the project,
  past or present.
- **Stable** — it will remain appropriate even if the implementation approach
  changes, the technology changes, or product terminology evolves slightly.
- **Verb-free** — it names a thing or domain capability, not an action or task.

### 2.3 Prohibited patterns

The following patterns are explicitly prohibited:

| Prohibited example | Reason |
| ------------------ | ------ |
| `feature-42` | Opaque; not human-readable |
| `new-feature` | Generic; provides no identity |
| `login-fix` | Describes an action, not a capability |
| `changes` | No semantic content |
| `final` | No semantic content |
| `final-v2` | No semantic content |
| `wip` | No semantic content |
| `misc-work` | No semantic content |
| `update-stuff` | No semantic content |
| `refactor` | Too vague and action-oriented |
| `bugfix` | No feature identity |
| `v2` | No feature identity |

PRINCIPLE: **Identity must describe the thing, not the temporary work being done to it.**

### 2.4 Acceptable patterns

Examples of valid slug patterns (these are illustrative, not project requirements):

| Example slug | Why it is valid |
| ------------ | --------------- |
| `otp-login-2fa` | Names the specific capability |
| `password-reset-email` | Specific enough to be unique; domain-oriented |
| `payment-retry-logic` | Names the specific capability |
| `audit-log-export` | Names a specific bounded feature |
| `rate-limit-login-endpoint` | Specific; names what is limited |

---

## 3. When the Slug Is Assigned

The slug must be assigned **at the moment a feature specification is created**.

```
Validated BRD-NNN
    ↓
Spec pre-writing gate (spec-prewriting-gate.md)
    ↓
SLUG ASSIGNED HERE ← before any other artefact is created
    ↓
Spec authored: .ai-context/specs/<feature-slug>.spec.md
```

A feature slug must NOT be assigned:
- Before a validated BRD entry exists.
- After a spec has already been drafted without one.
- As a retroactive rename once downstream artefacts exist.

Assigning a slug retroactively (after artefacts already reference the feature)
is a **traceability event** subject to the controlled rename process in §6.

---

## 4. Slug Propagation — The Complete Traceability Thread

The feature slug must appear consistently across every artefact in the feature's
delivery chain. This is what makes the slug the single traceability thread.

### 4.1 File names

| Artefact | Path |
| -------- | ---- |
| Specification | `.ai-context/specs/<feature-slug>.spec.md` |
| Plan | `.ai-context/plans/<feature-slug>.plan.md` |
| Tasks | `.ai-context/tasks/<feature-slug>.tasks.md` |
| Test cases | `.ai-context/test_cases/<feature-slug>.test_cases.md` |

### 4.2 Git and repository

| Artefact | Convention |
| -------- | ---------- |
| Feature branch | `feature/<feature-slug>` |
| Commits | `Implements <feature-slug>.T01` (task-ID traceability; see §9) |
| PR title | `[<feature-slug>] <human-readable description>` or equivalent traceable form |

### 4.3 Status and release

| Artefact | Convention |
| -------- | ---------- |
| Status board entry | Spec ID column uses `<feature-slug>` |
| Release notes | Reference `<feature-slug>` where artefact identity is relevant |
| Support and incident records | Reference `<feature-slug>` when traceable to a delivered feature |

### 4.4 ADR cross-references

ADRs are project-global (see §10) and use `ADR-NNNN` identifiers — not
`<feature-slug>.ADR01`. However, the ADR's **Feature / Context** field records
which feature slug triggered the decision. This preserves the cross-reference
from the ADR back to the feature, and allows the plan to reference the ADR.

### 4.5 Traceability chain diagram

```
BRD-NNN
    ↓
<feature-slug>  ←— ROOT IDENTITY assigned at spec creation
    │
    ├── .ai-context/specs/<feature-slug>.spec.md
    │       └── <feature-slug>.AC1, .AC2, .ACN
    │       └── <feature-slug>.API01, .API02, .APINN
    │       └── <feature-slug>.UT01, .UT02, .UTNN
    │
    ├── .ai-context/plans/<feature-slug>.plan.md
    │       └── references <feature-slug>.ACN, .APINN
    │
    ├── .ai-context/tasks/<feature-slug>.tasks.md
    │       └── <feature-slug>.T01, .T02, .TNN
    │           each mapping to .ACN / .APINN
    │
    ├── .ai-context/test_cases/<feature-slug>.test_cases.md
    │       └── references <feature-slug>.ACN, .UTNN
    │
    ├── feature/<feature-slug>           (branch)
    │
    ├── [<feature-slug>] title           (PR)
    │
    ├── status.md: Spec ID = <feature-slug>
    │
    └── ADR-NNNN (if applicable)
            └── Feature/Context: <feature-slug>
```

A reviewer or agent must be able to navigate from any artefact to any other
artefact in this chain using the slug alone — without guessing or paraphrasing.

---

## 5. Slug Uniqueness and Collision Rules

### 5.1 Uniqueness requirement

A slug must be unique across:
- All currently active feature specifications.
- All archived and deprecated feature specifications.
- All specifications that were superseded without being formally renamed.

Slugs do not expire. A retired slug occupies the identity space permanently.

### 5.2 Collision categories

| Collision type | Rule |
| -------------- | ---- |
| Exact duplicate (active + active) | **BLOCKED** — two features cannot share a slug |
| Exact duplicate (active + retired) | **BLOCKED** — retired slugs cannot be reused |
| Trivially different (e.g., `login-otp` vs `otp-login`) | **Requires human review** before approval |
| Semantically indistinguishable (different words, same meaning) | **Requires human review** before approval |
| Slug differing only in punctuation from an existing slug | **BLOCKED** |

### 5.3 Collision validation procedure

Before assigning a new slug, the following steps must be performed:

1. **Search active specs:** `ls .ai-context/specs/*.spec.md` and check filenames.
2. **Search archived specs:** Check `.ai-context/specs/archive/` (when the archive
   directory is created) for retired slugs.
3. **Search the slug registry:** Check `.ai-context/slug-registry.md` for the
   authoritative list of all slugs (active, retired, and archived).
4. **Search for semantic near-matches:** Review the slug list for slugs that
   describe the same capability with different words.
5. **Confirm uniqueness in status.md:** The Spec ID column records active slugs.

If a collision is found: **STOP. Assign a different slug before proceeding.**

### 5.4 Slug Registry

The slug registry is the single authoritative source of all slugs:

```
.ai-context/slug-registry.md
```

Every slug must be registered in this file when it is first assigned.
It records:

| Slug | Status | Assigned date | Spec file | Notes |
| ---- | ------ | ------------- | --------- | ----- |
| `<feature-slug>` | Active / Retired / Archived | `YYYY-MM-DD` | path | |

This is the minimum required mechanism for reliable slug uniqueness enforcement.
An agent or human can open this file and confirm in seconds whether a proposed
slug is available.

Do not leave slug assignment implicit or assume file-system search alone is
sufficient — archived specs may not be in the active directory.

---

## 6. Slug Immutability

### 6.1 Immutability rule

Once a feature slug has been established (i.e., any downstream artefact references
it), the slug **must not be changed** for any of the following reasons:

- Improved wording preference
- Implementation approach changed
- Technology changed
- Product terminology evolved
- Feature was split or expanded informally
- The spec author prefers a different filename
- The slug was spelled slightly differently than intended

Changing a slug after dependent artefacts exist is a **traceability event**, not
a casual rename. Traceability events require the controlled process in §6.2.

### 6.2 Controlled slug rename process

If a slug rename is determined to be genuinely necessary (rare), the following
process applies:

1. **Document the reason** in a governance note within the spec.
2. **Create a rename record** in `.ai-context/slug-registry.md` mapping the old
   slug to the new slug, with date and reason.
3. **Identify all stale references:** Search the repository for all occurrences
   of the old slug in specs, plans, tasks, test cases, status, architecture,
   ADRs, and prompt history.
4. **Update all artefact file names** simultaneously — not piecemeal.
5. **Update all internal references** to the old slug.
6. **Update the branch name** (this creates a new branch from the old branch;
   the old branch name must be noted in the rename record for history).
7. **Update status.md** to reflect the new slug.
8. **Record the rename in prompt_history.md** with before/after slug values.

The old slug must be recorded in `slug-registry.md` as `Renamed to: <new-slug>`.
It must NOT be made available for reuse.

Historical commit history referencing the old slug is acceptable and does not
need to be rewritten — Git history is the immutable audit trail.

---

## 7. Slug Retirement and Archival

### 7.1 When a slug retires

A slug retires when the feature reaches one of these terminal lifecycle states:

- `Deprecated`
- `Superseded`
- `Archived`

A released feature (`Released (vX.Y.Z)`) does not retire its slug — released
features remain identifiable by their slug indefinitely.

### 7.2 Archival process

When a feature is deprecated or superseded:

1. Update the spec's **Status** field to `Deprecated` or `Superseded`.
2. Move or retain the spec file — do **not** delete it.
3. Update `.ai-context/slug-registry.md` to mark the slug as `Retired`.
4. Record the date of retirement.
5. Update `status.md` to remove the spec from the Active Specs table (or add
   a Retired/Archived section if beneficial).

### 7.3 Retired slug reuse prohibition

A retired slug **must never be reused**, even if:
- The original feature is completely gone.
- The new feature would be in the same domain.
- Years have passed.
- The project has changed ownership.
- No engineer remembers the original feature.

The retired slug occupies the identity space permanently. Reusing it creates
a situation where old references (in Git history, release notes, support records,
ADRs, and incident reports) would refer to a different feature, destroying the
historical audit trail.

---

## 8. Stable Sub-Identifiers

### 8.1 The identifier hierarchy

The feature slug is the ROOT identifier. All feature-scoped sub-identifiers are
formed by appending a typed prefix and sequence number to the slug.

| ID type | Format | What it identifies |
| ------- | ------ | ------------------ |
| Acceptance criteria | `<slug>.AC1`, `<slug>.AC2` … | Individual acceptance criteria in the spec |
| API contract entries | `<slug>.API01`, `<slug>.API02` … | Individual API endpoint/operation contracts |
| Unit test cases | `<slug>.UT01`, `<slug>.UT02` … | Spec-derived unit test mappings |
| Tasks | `<slug>.T01`, `<slug>.T02` … | Ordered implementation tasks |

### 8.2 Numbering conventions

- Numbering starts at `1` for AC/T and `01` for API/UT (zero-padded two digits).
- IDs are assigned sequentially in the order items are first introduced.
- Numbering must not be reset between spec revisions.
- IDs must not be reused within a feature even if the original item is retired.

### 8.3 ADR identifiers — the explicit exception

ADRs are **project-global** — not slug-scoped:

```
ADR-0001
ADR-0002
ADR-0003
```

An ADR may apply across multiple feature slugs and may outlive individual
features. Therefore, ADR IDs must NOT be formatted as `<slug>.ADR01`.

An ADR records which feature slug(s) triggered it in the ADR's **Feature / Context**
field, preserving the relationship without scoping the ADR identity to the feature.

---

## 9. Sub-Identifier Immutability

### 9.1 IDs are stable identities, not positional labels

A stable ID is an identity, not a position in a list.

**IDs must not shift because of list reordering.**

If a spec originally contains:

```
<slug>.AC1 — First criterion
<slug>.AC2 — Second criterion
<slug>.AC3 — Third criterion
```

and `<slug>.AC2` is subsequently removed, the rule is:

- `<slug>.AC1` **retains** its identity and meaning.
- `<slug>.AC3` **retains** its identity and meaning.
- `<slug>.AC2` is **retired** — it does not shift to a new item.
- The gap (no `AC2` in the active list) is intentional and must not be filled.

A spec reviewer seeing `<slug>.AC3` in a task must be able to look up `<slug>.AC3`
in the spec and find the same item that was there when the task was written —
regardless of list position.

### 9.2 Content change vs identity change

| Operation | Classification | Rule |
| --------- | -------------- | ---- |
| Editing the wording of an AC without changing its meaning | Content change | Retain same ID; note revision in spec |
| Reordering ACs in the spec without changing content | Content change | **Do NOT renumber** |
| Adding a new AC | Identity creation | Assign next sequential ID (e.g., `AC4` if AC1–AC3 exist) |
| Removing an AC that is no longer required | Identity retirement | Retire the ID; do **not** reassign it |
| Rewriting an AC so fundamentally that its meaning changes | Identity change | Retire the old ID; assign a new ID; document in revision notes |
| Splitting one AC into two | Identity creation | Retire the original ID; create two new IDs with the next sequential numbers |
| Merging two ACs into one | Identity creation | Retire both original IDs; create one new ID with the next sequential number |

### 9.3 Retired sub-identifier handling

A retired sub-identifier (e.g., `<slug>.AC2`) must:
- Be documented in the spec's revision history or open items section.
- Never be reassigned to a different criterion within the same feature.
- Remain interpretable — the spec should note what it was and why it was retired.

References from tasks, tests, or other artefacts to a retired sub-identifier are
**broken references** that must be resolved (see §11).

---

## 10. Identity Namespaces

The repository uses three distinct identity namespaces. These must not be confused.

| Namespace | Format | Scope | Governed by |
| --------- | ------ | ----- | ----------- |
| Feature slug | `<kebab-case-slug>` | One feature | This document |
| Acceptance criteria | `<slug>.AC1` … | One feature | This document |
| API contract entries | `<slug>.API01` … | One feature | This document |
| Unit test cases | `<slug>.UT01` … | One feature | This document |
| Tasks | `<slug>.T01` … | One feature | This document |
| Business requirements | `BRD-NNN` | Project | `.ai-context/BRD.md` |
| Architectural decisions | `ADR-NNNN` | Project-global | `.agent/rules/int-sdd-architecture.md` |

**Cross-namespace reference rule:**

A task (`<slug>.T01`) may reference an AC (`<slug>.AC1`) — this is expected.
A plan may reference an ADR (`ADR-0003`) — this is expected.
An ADR may reference which feature slug triggered it — this is expected.

However, a task from feature A must NOT silently reference a task from feature B
(`featureB.T02`) without an explicit, governed cross-feature dependency documented
in both specs.

---

## 11. Cross-Reference and Validity Rules

### 11.1 A valid reference

A reference such as `<slug>.AC3` is valid when:
- The slug exists in the slug registry as active.
- The spec file `.ai-context/specs/<slug>.spec.md` exists.
- The AC with ID `.AC3` exists in that spec and is not retired.

### 11.2 A broken reference

A reference is broken when:
- The slug is not in the registry (unknown slug).
- The spec file does not exist at the expected path.
- The ID (`AC3`) does not appear in the spec (missing ID).
- The ID was retired without the reference being updated.
- The slug prefix of the ID does not match the feature being described.

### 11.3 Duplicate ID detection

A duplicate ID exists when two items within the same feature share the same
identifier (e.g., two ACs with the same `.AC3` label). This is a BLOCKING error.

### 11.4 Cross-feature reference governance

A task or test that references an ID from a different feature (e.g., feature A's
task referencing `featureB.AC2`) is a cross-feature reference. This is:
- **Acceptable** when the dependency is explicitly documented in both feature specs.
- **BLOCKING** when the reference is implicit and undocumented.

### 11.5 Malformed ID detection

An ID is malformed when it does not follow the correct format:
- `<slug>-AC3` (hyphen instead of dot) — MALFORMED
- `<slug>.ac3` (lowercase type code) — MALFORMED
- `<SLUG>.AC3` (uppercase slug) — MALFORMED
- `<slug>.ADR01` (ADR scoped to feature) — INVALID (ADR must be project-global)
- `AC3` (no slug prefix) — MALFORMED

---

## 12. Prompt-by-Identity Governance

### 12.1 The principle

When a stable identifier already exists for an artefact, use it.

Prefer:
```
Implement <feature-slug>.T03. Satisfy <feature-slug>.AC3. Follow <feature-slug>.API02.
```

Over:
```
Implement the rate-limiting task.
```

### 12.2 Why this matters

Surrounding prose changes. Implementation plans evolve. Task descriptions are
reworded. An identity is stable.

If an agent is instructed to work on "the rate-limiting task" and there are
two tasks that could be described as rate-limiting, the instruction is ambiguous.
If the agent is instructed to work on `payment-retry.T03`, there is exactly one
addressable target.

### 12.3 When to use stable IDs

| Situation | Stable ID preference |
| --------- | -------------------- |
| Referring to an AC in a task or test | **Always** use `<slug>.ACN` |
| Referring to an API contract in a plan | **Always** use `<slug>.APINN` |
| Referring to a task in a commit or PR | **Always** use `<slug>.TNN` |
| Referring to an architectural decision | **Always** use `ADR-NNNN` |
| Referring to a business requirement | **Always** use `BRD-NNN` |
| Prompting an agent to work on a task | **Prefer** `<slug>.TNN` over prose description |

### 12.4 Limit of the principle

This principle does not require that every conversational prompt contain every
possible ID. The principle is:

**USE IDENTITY WHEN AN ARTEFACT ALREADY HAS A STABLE ID.**

In early discovery conversations where IDs have not yet been assigned, prose is
acceptable. Once IDs exist, prose alone becomes imprecise.

---

## 13. Automated Checks vs Human Approval

An automated naming/identifier check such as the output of
`.agent/workflows/naming-identifier-check.md` may produce:

```
NAMING CHECK PASSED
```

or

```
NAMING CHECK — BLOCKED: <findings>
```

This is analysis output — not governance approval.

A passed naming check does not mean:
- The slug has been officially approved.
- The spec has passed Gate 1.
- Human review of slug quality has occurred.

The naming check is a pre-condition for proceeding, not a substitute for the
human Gate 1 review. If a naming issue requires human judgment (e.g., two slugs
that are arguably semantically indistinguishable), it must be escalated to a
human decision before proceeding.

---

## 14. Integration with Existing Lines

This rule complements and formalizes the naming provisions already established:

| Existing rule | Where naming is addressed | Line 9 relationship |
| ------------- | ------------------------- | ------------------- |
| `int-sdd-specification-standard.md` §2 | Slug format rules | Line 9 makes these authoritative and extends them |
| `int-sdd-architecture.md` §12–13 | One branch per spec; branch naming | Line 9 confirms and cross-references; does not duplicate |
| `constitution.md` §Feature traceability | Slug propagation chain | Line 9 formalizes this definitively |
| `repository-check.md` §1–2 | Slug consistency check | Line 9 extends with collision, retirement, ID drift rules |
| `gate-1-review.md` §Identity | Reviewer identity, not slug identity | No conflict |

Line 9 is the canonical source. Other documents should reference this rule for
slug and ID governance, not duplicate it.
