# Naming and Identifier Check Workflow

## Authority

This workflow validates the naming and identifier discipline for a feature
under INT SDD Line 9.

Governing rule: `.agent/rules/int-sdd-naming.md`
Slug registry: `.ai-context/slug-registry.md`
Architecture / repository standards: `.agent/rules/int-sdd-architecture.md`

---

## When to Run

Run this workflow:

1. **At slug assignment** — before first creating the spec file.
2. **At spec submission for Gate 1** — as part of `spec-review.md`.
3. **At task generation** — to confirm sub-identifier consistency.
4. **At Gate 2** — as part of the overall review.
5. **At deprecation/archival** — to confirm retirement is recorded.

---

## Inputs Required

- Proposed or existing feature slug: `<feature-slug>`
- Spec file (if exists): `.ai-context/specs/<feature-slug>.spec.md`
- Plan file (if exists): `.ai-context/plans/<feature-slug>.plan.md`
- Tasks file (if exists): `.ai-context/tasks/<feature-slug>.tasks.md`
- Test cases file (if exists): `.ai-context/test_cases/<feature-slug>.test_cases.md`
- Slug registry: `.ai-context/slug-registry.md`
- Branch name (if exists): `feature/<feature-slug>` or other

---

## OUTPUT FORMAT

Produce either:

```
NAMING CHECK PASSED
```

or:

```
NAMING CHECK — BLOCKED
```

with structured findings using the format in the Findings section.

A PASSED result is pre-condition analysis — not human approval.
Slug quality issues that require human judgment must be escalated.

---

## Part 1 — Slug Existence and Assignment Timing

- [ ] **[BLOCKING]** A feature slug has been assigned.
- [ ] **[BLOCKING]** The slug was assigned when the spec was created — not
      retroactively applied after content was written without an identity.
- [ ] The slug is present in `.ai-context/slug-registry.md`.
- [ ] **[BLOCKING if absent]** If the slug is NOT in the registry: register it
      before proceeding or treat as a BLOCKING finding.

---

## Part 2 — Slug Format Validity

- [ ] **[BLOCKING]** Slug uses only lowercase letters, digits, and hyphens
      (`[a-z0-9-]`). No uppercase, no underscores, no spaces, no dots.
- [ ] **[BLOCKING]** Slug is kebab-case (words separated by single hyphens).
- [ ] **[BLOCKING]** Slug is approximately 3–5 words in length.
- [ ] **[BLOCKING]** Slug is human-readable — a competent engineer can understand
      the feature from the slug alone.
- [ ] **[BLOCKING]** Slug is specific enough to be unique for the project lifetime.
- [ ] **[BLOCKING]** Slug does not use any prohibited pattern:
      `feature-42`, `new-feature`, `login-fix`, `changes`, `final`, `final-v2`,
      `wip`, `misc-work`, `update-stuff`, `refactor`, `bugfix`, `v2`, or equivalent.
- [ ] Slug is domain/capability-oriented, not action/task-oriented (verb-free).

---

## Part 3 — Slug Uniqueness and Collision Check

**Check the slug registry at `.ai-context/slug-registry.md`:**

- [ ] **[BLOCKING]** The slug does not appear in the Active Slugs table under a
      different spec.
- [ ] **[BLOCKING]** The slug does not appear in the Retired / Archived / Renamed
      Slugs table.
- [ ] **[Requires human review]** No other slug in the registry describes the same
      capability with different words (semantic near-match).
- [ ] **[Requires human review]** No other slug differs only trivially in word order
      or punctuation from this slug.

**Additional file-system check:**

- [ ] `ls .ai-context/specs/` does not contain another `<same-slug>.spec.md`.
- [ ] `ls .ai-context/specs/archive/` (if it exists) does not contain the slug.

---

## Part 4 — Slug Immutability Check

_Only applicable when the spec already exists and artefacts already reference it._

- [ ] **[BLOCKING]** The slug has not been changed since downstream artefacts
      (plan, tasks, tests, branch) were created without following the controlled
      rename process in `.agent/rules/int-sdd-naming.md` §6.2.
- [ ] If a rename occurred: the slug registry records the old slug as `Renamed` and
      the rename record is present with date, reason, and new slug value.

---

## Part 5 — Artefact Filename Consistency

Check that the slug appears correctly in every artefact filename.

| Artefact | Expected path | Exists? | Slug matches? |
| -------- | ------------- | ------- | ------------- |
| Spec | `.ai-context/specs/<feature-slug>.spec.md` | ☐ | ☐ |
| Plan | `.ai-context/plans/<feature-slug>.plan.md` | ☐ / N/A | ☐ / N/A |
| Tasks | `.ai-context/tasks/<feature-slug>.tasks.md` | ☐ / N/A | ☐ / N/A |
| Test cases | `.ai-context/test_cases/<feature-slug>.test_cases.md` | ☐ / N/A | ☐ / N/A |

- [ ] **[BLOCKING]** Spec file exists at the correct path with the slug in the filename.
- [ ] **[BLOCKING if exists]** Plan filename slug matches the spec slug.
- [ ] **[BLOCKING if exists]** Tasks filename slug matches the spec slug.
- [ ] **[BLOCKING if exists]** Test-case filename slug matches the spec slug.

**Internal header check:**

- [ ] The `## Spec ID` field in the spec contains `<feature-slug>` (verbatim match).
- [ ] **[BLOCKING if mismatch]** The slug in the Spec ID header must match the
      filename slug exactly.

---

## Part 6 — Branch Naming Check

- [ ] **[BLOCKING]** If a branch exists: it follows the convention
      `feature/<feature-slug>`.
- [ ] **[BLOCKING]** Branch name slug matches the spec slug exactly.
- [ ] Branch is not named with a prohibited vague pattern.

---

## Part 7 — PR Traceability Check

_Only applicable when a PR exists._

- [ ] **[BLOCKING]** PR title contains the feature slug in a traceable form
      (e.g., `[<feature-slug>]` or equivalent).
- [ ] PR body references task IDs from the correct feature slug
      (`<feature-slug>.T01`, etc.).
- [ ] PR does not reference task IDs from a different feature without an explicit
      governed cross-feature dependency.

---

## Part 8 — Status Board Traceability

- [ ] The spec appears in `.ai-context/status.md` Active Specs table with its
      slug as the Spec ID.
- [ ] **[BLOCKING if active but not listed]** An active feature must appear in
      the status board with its correct slug.

---

## Part 9 — Sub-Identifier Validity Check

_For each of the following ID types: verify all instances in the spec._

### Acceptance Criteria IDs (`<slug>.ACN`)

- [ ] **[BLOCKING]** Each AC ID uses the format `<slug>.AC` followed by a
      positive integer (e.g., `.AC1`, `.AC2`).
- [ ] **[BLOCKING]** No two ACs share the same ID within the spec.
- [ ] **[BLOCKING]** AC IDs use the correct slug prefix (matches the spec's Spec ID).
- [ ] AC IDs start at `AC1` and are sequential without unexplained gaps.
  - If gaps exist (e.g., `AC1`, `AC3` with no `AC2`): the gap must be explained
    in the spec's revision history as a retired ID.
- [ ] AC IDs have not shifted because of list reordering (compare with prior
      revision if available).

### API Contract IDs (`<slug>.APINN`)

- [ ] **[BLOCKING]** Each API contract ID uses the format `<slug>.API` followed
      by a zero-padded two-digit integer (e.g., `.API01`, `.API02`).
- [ ] **[BLOCKING]** No two API entries share the same ID within the spec.
- [ ] **[BLOCKING]** API IDs use the correct slug prefix.
- [ ] API IDs start at `API01` and are sequential without unexplained gaps.
- [ ] API section states `Not applicable` when no API surface exists (rather than
      being silently absent).

### Unit Test Case IDs (`<slug>.UTNN`)

- [ ] **[BLOCKING]** Each unit test ID uses the format `<slug>.UT` followed by a
      zero-padded two-digit integer (e.g., `.UT01`, `.UT02`).
- [ ] **[BLOCKING]** No two UT entries share the same ID within the spec.
- [ ] **[BLOCKING]** UT IDs use the correct slug prefix.
- [ ] Each UT ID maps to an existing, non-retired AC ID.

### Task IDs (`<slug>.TNN`)

- [ ] **[BLOCKING]** Each task ID uses the format `<slug>.T` followed by a
      zero-padded two-digit integer (e.g., `.T01`, `.T02`).
- [ ] **[BLOCKING]** No two tasks share the same ID within the tasks file.
- [ ] **[BLOCKING]** Task IDs use the correct slug prefix (matches the spec slug).
- [ ] Each task's traceability column references existing, non-retired AC or API IDs.
- [ ] Task IDs start at `T01` and are sequential without unexplained gaps.
  - Gaps must be explained in the task file as retired task IDs.

---

## Part 10 — Cross-Reference Validity

- [ ] **[BLOCKING]** All `<slug>.ACN` references in the plan, tasks, and tests
      correspond to actually existing, non-retired ACs in the spec.
- [ ] **[BLOCKING]** All `<slug>.APINN` references in the plan, tasks, and tests
      correspond to actually existing, non-retired API contract entries in the spec.
- [ ] **[BLOCKING]** All `<slug>.UTNN` references in test files correspond to
      actually existing, non-retired UT IDs in the spec.
- [ ] **[BLOCKING]** All `<slug>.TNN` references in commit messages or PRs
      correspond to actually existing, non-retired task IDs in the tasks file.
- [ ] **[Requires review]** Any cross-feature reference (a task referencing an ID
      from a different slug) is explicitly documented and governed.
- [ ] **[BLOCKING]** No artefact references an ID with an incorrect slug prefix
      (e.g., feature A's task referencing `featureB.AC3` without documented dependency).

---

## Part 11 — ADR Identity Check

- [ ] **[BLOCKING]** No artefact within the feature uses `<slug>.ADR01` or similar
      slug-scoped ADR ID format. ADR IDs must be project-global (`ADR-NNNN`).
- [ ] ADR references in the plan and spec use the `ADR-NNNN` format.
- [ ] The ADR's **Feature / Context** field records the feature slug (this is the
      correct direction of the cross-reference).

---

## Part 12 — Retired Slug Reuse Check

When a new slug is being proposed:

- [ ] **[BLOCKING]** The proposed slug does not appear in the Retired / Archived /
      Renamed section of `.ai-context/slug-registry.md`.
- [ ] **[BLOCKING]** The proposed slug is not the old name of a renamed feature.

When deprecating/archiving an existing feature:

- [ ] Slug registry has been updated to mark the slug as `Retired` or `Archived`.
- [ ] Spec status has been updated to `Deprecated` or `Superseded`.
- [ ] Status board has been updated.

---

## Part 13 — Prompt-by-Identity Readiness

_Verify that artefacts are addressed using stable IDs where those IDs exist._

- [ ] Plan references `<slug>.ACN` and `<slug>.APINN` IDs explicitly (not just prose).
- [ ] Tasks reference `<slug>.ACN` and `<slug>.APINN` IDs in the traceability column.
- [ ] Test cases reference `<slug>.ACN` IDs explicitly.
- [ ] Commit messages use `<slug>.TNN` for task-ID traceability.
- [ ] ADR references use `ADR-NNNN` (not informal descriptions like "the auth decision").
- [ ] BRD references use `BRD-NNN` (not informal descriptions).

---

## Findings Format

For each issue found, use this format:

```
ID:                  NI-<AREA>-NNN
Severity:            BLOCKING | REQUIRES_HUMAN_REVIEW | NON-BLOCKING
Area:                <area code>
Observation:         <what was found>
Why it matters:      <traceability or governance consequence>
Evidence:            <specific text, filename, or absence>
Required action:     <what must be done before proceeding>
```

Area codes:

| Code | Area |
| ---- | ---- |
| SLG | Slug format or existence |
| COL | Collision or uniqueness |
| IMM | Immutability violation |
| FNM | Filename consistency |
| BRN | Branch naming |
| PRT | PR traceability |
| STS | Status board |
| ACI | Acceptance criteria IDs |
| API | API contract IDs |
| UTI | Unit test IDs |
| TSK | Task IDs |
| XRF | Cross-reference validity |
| ADR | ADR identifier |
| RET | Retired slug reuse |
| PBI | Prompt-by-identity |

---

## Failure Scenarios Governed by This Workflow

| Scenario | Expected outcome | Check part |
| -------- | ---------------- | ---------- |
| Two active features propose same slug | BLOCKING — COL | Part 3 |
| New feature reuses a retired slug | BLOCKING — RET | Parts 3, 12 |
| Spec file renamed; slug elsewhere unchanged | BLOCKING — FNM | Part 5 |
| AC list reordered; IDs shifted | BLOCKING — ACI | Part 9 |
| Task references `<slug>.AC9` but AC9 does not exist | BLOCKING — XRF | Part 10 |
| Feature A task references `featureB.T02` without governance | Requires review — XRF | Part 10 |
| PR contains no traceable feature identity | BLOCKING — PRT | Part 7 |
| ADR named `<slug>.ADR01` | BLOCKING — ADR | Part 11 |
| Spec prose heavily edited; stable IDs unchanged | PASSED — identity survives | Parts 5, 9 |
| Feature deprecated; slug added to retired table | PASSED — correctly retired | Part 12 |

---

## Critical Reminders

1. **This workflow produces analysis — not governance approval.**
2. **A PASSED result is a pre-condition, not a Gate 1 substitute.**
3. **Human judgment is required for semantic near-match slug review.**
4. **Retired slugs are permanently unavailable — no exceptions.**
5. **IDs are identities, not positions — do not renumber on reorder.**
