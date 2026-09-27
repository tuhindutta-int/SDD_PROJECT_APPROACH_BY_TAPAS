# INT SDD Architecture and Repository Standards

## Authority

This is the authoritative repository rule for architecture governance and
repository discipline. It applies to all plans, tasks, implementations,
and repository operations executed under INT SDD.

Related artefacts:
- Architecture check workflow: `.agent/workflows/architecture-check.md`
- ADR assessment workflow: `.agent/workflows/adr-check.md`
- Repository check workflow: `.agent/workflows/repository-check.md`
- Living architecture document: `.ai-context/architecture.md`
- ADR store: `.ai-context/decisions/ADR-NNNN-<slug>.md`
- Plan template: `.ai-context/plans/_template.plan.md`
- Constitution: `.ai-context/constitution.md`
- Lifecycle: `.ai-context/lifecycle.md`

---

## PART A — ARCHITECTURE GOVERNANCE

---

## 1. Mandatory Plan → Constitution Check

**This is a hard rule. It cannot be skipped or deferred.**

Every plan must be checked against `.ai-context/constitution.md` before
task generation proceeds.

Required derivation chain:

```
Approved Spec
    ↓
Plan (derived from approved spec)
    ↓
Constitution Check  ← HARD GATE
    ↓
Architecture Check  ← HARD GATE
    ↓
Task Generation
```

The following path is **NOT PERMITTED**:

```
Spec → Plan → Tasks           (skips constitution and architecture checks)
Spec → Code                   (skips plan entirely)
Spec → Architecture → Tasks   (skips plan review)
```

Silence is not compliance. If the constitution contains a meaningful rule
and the plan is silent on how that rule is satisfied or intentionally not
applicable: **FLAG IT. Do not proceed.**

Constitution areas that must be checked against every plan:

- Testing discipline (test-first, AC derivation, RED evidence requirement)
- Security posture (security-relevant behaviour must be explicit)
- Architectural constraints (no unspecified technology or topology)
- Non-functional baselines (performance, availability, compliance where defined)
- Versioning rules
- Repository rules (no secrets/PII, no AI attribution)
- Agent authority boundary (no plan may invent product requirements)

A plan that violates any constitution rule is a Gate 1 / plan-stage blocker.
Do not defer constitution violations to Gate 2.

---

## 2. Plan Must Name Integration Points

Where a feature touches external or internal integrations, the plan must
explicitly identify:

- What integration is touched (external API, internal service, event system,
  data store, auth system, third-party component, platform capability)
- What behaviour changes at that integration
- What contract is relevant
- What dependency exists
- What impact is expected on the integration and any consumers

Do not leave significant integration decisions implicit for the implementation
agent to discover at coding time.

When no integration is affected: state explicitly **Not applicable** in the
plan's Integration Points section.

Do not invent integrations that are not specified by the approved spec or
the current architecture.

---

## 3. Plan Must Name the Data Model Impact

Where a feature affects persistent or transient data, the plan must explicitly
identify:

- Data model or schema changes
- Entity or relationship changes
- Migration implications
- Persistence changes
- Transient or session-state changes

When no data model change is required: state explicitly **Not applicable** in
the plan's Data Model section.

Do not leave meaningful data-model decisions implicit. Do not allow the
implementation agent to guess the schema.

---

## 4. Plans Must Explicitly State Deferred Items

Every non-trivial plan must explicitly list what is deferred — not silently
omitted.

Deferred items are out-of-scope items that could be mistaken for in-scope
work. Examples may include:

- Future migration or rollback strategy
- Additional flows (e.g., backup payment method)
- Advanced analytics or reporting
- Broader platform refactoring
- Audit trails and administrative tooling

Purpose: prevent implementation from silently expanding into deferred scope.

At Gate 2, the reviewer must check that implementation did not scope-creep
into deferred items.

When nothing is deferred: state explicitly **None — no items deferred** in the
plan's Deferred Items section.

---

## 5. Significant Architectural Decisions Require an ADR

**INT definition of a significant architectural decision:**

> A decision is significant when reversing it later would cost more than one
> day of rework.

When a plan introduces a decision that meets this threshold, an ADR is required
before the affected implementation proceeds.

Examples of the decision category (not a project architecture list):

- Introducing a new datastore or persistence layer
- Introducing a new service or micro-service boundary
- Selecting a major integration approach or protocol
- Changing an existing service boundary
- Selecting a major data-management strategy
- Changing an existing system-wide pattern
- Changing state-management approach
- Introducing a significant external dependency

These are categories for the decision type — not approved project choices.
Do not read them as existing project architecture.

If the decision is not significant (reversal costs less than a day): an ADR
is not required; document the choice inline in the plan.

---

## 6. ADR Is the "Why" — Plan Is the "How"

```
Plan    = implementation approach (HOW)
ADR     = significant decision rationale (WHY we chose this approach)
```

An ADR must explain:

- **Context:** what situation prompted the decision
- **Decision:** what was decided
- **Alternatives considered:** other options and why they were not chosen
- **Consequences:** positive, negative, and trade-offs

Do not use ADRs as a dumping ground for every minor implementation detail.
Do not put a significant architectural decision only in chat history.
Do not put a significant architectural decision only in a commit message.

The plan should reference any relevant ADR. The ADR should not duplicate
the plan's implementation detail.

---

## 7. ADR Identifiers Are Project-Global

ADR identifiers use project-global sequential numbering:

```
ADR-0001
ADR-0002
ADR-0003
```

Do not scope ADR numbering by feature. An ADR may outlive a single feature
and apply across multiple features.

Filename convention:

```
.ai-context/decisions/ADR-NNNN-<decision-slug>.md
```

Example (hypothetical, not a real project decision):

```
.ai-context/decisions/ADR-0001-event-sourcing-for-audit.md
```

Do not create ADRs during governance setup. ADRs are created when real
architectural decisions are made.

---

## 8. `architecture.md` Is a Living Document

`.ai-context/architecture.md` is the current-state system-design record.

It must reflect current architecture, including:

- Integration points currently in use
- Significant decisions currently in force
- Important service and component boundaries
- Data layer and persistence in use
- Authentication and authorization model in use
- Infrastructure and deployment model in use

When a plan introduces:

- A new integration
- A new datastore or persistence layer
- A significant architectural decision
- A meaningful service or component boundary change

`architecture.md` must be updated as part of the same SDD change — not
deferred to a later clean-up session.

**Stale architecture documentation is a governance risk.** A future agent
session reading stale `architecture.md` may produce incorrect plans, incorrect
ADR assessments, and incorrect task scope.

The Architecture Check workflow (`.agent/workflows/architecture-check.md`)
must verify whether an `architecture.md` update is required and flag it as
a required action where applicable.

---

## 9. Architecture Change Propagation

When a significant architectural decision changes, identify all affected
downstream artefacts before proceeding:

```
ADR change
    ↓
architecture.md update
    ↓
Specs, plans, and tasks affected by the changed decision
    ↓
Tests and implementation where required
```

Do not silently allow downstream artefacts to depend on obsolete architecture.
Do not automatically rewrite unrelated feature artefacts.
First identify the affected scope, then require human decision before updating.

---

## 10. No Plan May Bypass the Approved Spec

The plan derives from the approved spec. The plan defines HOW — not WHAT.

The plan must not:

- Introduce new product requirements
- Redefine acceptance criteria
- Invent API behaviour not defined in the spec
- Invent data-model decisions that represent product choices
- Silently choose product-level behaviour that was unspecified

If the plan cannot be written without inventing product behaviour:
**STOP. Return to the spec. Update the spec through the correct Gate 1 process.**

---

## 11. Trivial Change Exception

Genuine trivial changes — configuration bumps, formatting, provably zero-
behaviour changes — may bypass the full architecture governance workflow.

Do not use this exception for:

- New features presented as "just a config change"
- Behaviour-affecting refactors
- Dependency changes with architectural implications
- Any change where architectural impact is unclear

When classification is unclear: **FLAG IT for a human decision.**
The default classification is non-trivial.

---

## PART B — REPOSITORY GOVERNANCE

---

## 12. One Feature Branch Per Spec

**ONE FEATURE SPEC = ONE FEATURE BRANCH**

The branch must trace directly to the same feature slug used across all artefacts.

Traceability chain for a feature `<feature-slug>`:

```
.ai-context/specs/<feature-slug>.spec.md      ← feature contract
.ai-context/plans/<feature-slug>.plan.md      ← technical approach
.ai-context/tasks/<feature-slug>.tasks.md     ← execution sequence
.ai-context/test_cases/<feature-slug>.test_cases.md  ← test cases
feature/<feature-slug>                        ← branch
<feature-slug>.T01, .T02 …                   ← commit traceability
```

This preserves a bounded, auditable scope between spec ↔ branch ↔ PR ↔ tasks.

---

## 13. Branch Naming Convention

Use slug-traceable branch names:

| Purpose | Convention | Example |
| ------- | ---------- | ------- |
| Feature delivery | `feature/<feature-slug>` | `feature/payment-retry` |
| Bug fix | `fix/<issue-slug>` | `fix/otp-counter-reset` |
| Hotfix | `hotfix/<incident-slug>` | `hotfix/HOTFIX-auth-bypass` |

Do not use vague branch names for feature delivery:

Prohibited: `dev`, `test`, `new-feature`, `changes`, `final`, `final-v2`,
`tuhin-work`, `experiment`, `wip`

The identifier in the branch name must remain traceable back to the
corresponding SDD artefact.

Do not create actual feature branches during governance setup.

---

## 14. Branch Scope Must Be Bounded

A feature branch must contain changes bounded by the corresponding
specification. Do not combine in one feature branch:

- Multiple independent features
- Unrelated refactors
- Unrelated bug fixes
- Architecture experiments
- Cleanup work not in the approved spec

If a PR cannot be reviewed against the spec's acceptance criteria in a
single sitting: the feature is likely scoped too broadly. The correct
action is to split the spec — not to allow an unreviewable PR.

---

## 15. Merge Strategy — Squash-Merge to Main

**Repository convention: squash-merge to main.**

Purpose: AI-generated incremental commits (often many per task) are
consolidated into one human-paced logical merge unit. The squash-merge
becomes the auditable unit, not individual agent commits.

SDD artefacts (task IDs, spec references), the Gate 2 review record, and
the final squash-merge commit are the primary traceability mechanism —
not the individual incremental commits.

Do not perform actual merges during governance setup.

---

## 16. AI Attribution Is Prohibited in Commits

Do NOT add AI attribution to commit messages, PR descriptions, code
comments, or implementation notes.

Prohibited patterns:

- `Generated by Claude`
- `Generated by Codex`
- `AI-assisted`
- `Written by AI`
- `Co-authored by AI`
- `[AI]`

Task-ID traceability is allowed and encouraged:

```
Implements payment-retry.T03
Fixes otp-counter-reset.T01
```

This identifies the engineering change without transferring ownership
to the tool.

---

## 17. Human Accountability Remains

Repository governance must remain consistent with Line 4 (Non-Negotiable Rules):

> A human is accountable for every merged line.

A branch being generated by AI does not change ownership. A passing Gate 1
does not eliminate human responsibility. A passing test suite does not
eliminate human responsibility.

Gate 2 review is required before merge, regardless of AI generation level.

---

## 18. PR Traceability

Where the repository uses pull requests, the PR must be traceable:

```
PR title      → feature slug → spec
PR body       → plan reference, task IDs, AC verification evidence
PR review     → Gate 2 human review evidence
PR merge      → squash-merge commit with task-ID traceability
```

PR title example (not mandatory exact syntax — traceability is the key):

```
[payment-retry] Implement retry handling
```

Do not enforce a literal PR title format unless already approved for the
project. The key requirement is that the PR traces to exactly one feature spec.

---

## 19. `.agent/` and `.ai-context/` Repository Placement

Both directories belong in the **internal engineering repository**.

Whether they are shipped to a client-facing repository is an
engagement-level decision governed by the Statement of Work.

Default:

> **Exclude from client-facing repository unless the SOW explicitly permits it.**

Do not expose:

- Internal agent control rules
- Prompt history
- Internal engineering context
- Review findings

…to client-facing repositories by accident.

This is not permission to delete `.agent/` or `.ai-context/` from the
internal repository. They must remain version-controlled in the internal repo.

---

## 20. Repository Structure

Preserve the following structural distinction:

| Directory | Purpose |
| --------- | ------- |
| `.agent/` | AI control plane — rules, workflows, agent context boundary |
| `.ai-context/` | Project knowledge base — requirements, specs, plans, architecture, decisions, status |
| `src/` (or equivalent) | Application source code (project-defined) |
| `tests/` (or equivalent) | Executable tests (project-defined) |
| `docs/` (or equivalent) | Operational, user, and developer documentation (project-defined) |

Do not move SDD artefacts into arbitrary folders for convenience.
Do not merge `.agent/` and `.ai-context/` into one directory.

---

## 21. Architecture and Repository Scope Summary

The architecture and repository governance model answers:

| Question | Answer |
| -------- | ------ |
| When is architecture reviewed? | At the Architecture Check stage, after plan, before tasks |
| What must a plan explicitly describe? | Integration points, data model, deferred items, constitution compliance |
| What makes a decision ADR-worthy? | Reversal would cost more than one day of rework |
| Where are ADRs recorded? | `.ai-context/decisions/ADR-NNNN-<slug>.md` |
| How does `architecture.md` remain current? | Updated as part of every SDD change that introduces architectural impact |
| How are specs/plans/tasks traceable through Git? | Feature slug used in branch, commits (task IDs), and PR |
| How is repository scope kept aligned with one feature? | One branch per spec; branch scope enforced; PR reviewable against spec |
| What prevents opportunistic architecture? | `architecture-check.md` blocks task generation until architecture is assessed |
