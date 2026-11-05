# INT SDD — Master Governance State & Repository Handover

> Audit Date: 2026-10-12 | Commit at audit: 71d3e1a (HEAD, branch main)
> Purpose: Authoritative handover for any future engineer or AI agent entering this repository without prior conversation context.

## 1. Document Purpose

This document is the single master reference for the INT SDD governance foundation. It answers: what the repository is, which Lines were implemented and with what artefacts, the file inventory, authority hierarchy, cross-file dependencies, the lifecycle and traceability model, what is deferred vs absent, gaps found, and how a new engineer or AI agent should operate.

This is NOT a marketing document. It uses TBD, gap, and not verified where warranted.

---

## 2. Source-of-Truth Hierarchy

Level 1: INT SDD BluePrint V1.0.pdf (governing philosophy)
Level 2: Actual repository files (read directly — primary evidence)
Level 3: Git commit history (historical sequence)
Level 4: status.md and prompt_history.md (corroborating history)
Level 5: Prior completion reports (overridden by Level 2 evidence)

Every file referenced in this document was directly read or its existence verified. No claims are made from conversation memory.

### Commits Confirmed Against Actual git log

| Commit   | Summary                                          | Branch |
|----------|--------------------------------------------------|--------|
| 71d3e1a  | Establish INT SDD Line 11 QA and Security        | main   |
| 5e02e79  | Establish INT SDD Line 10 Development Discipline | main   |
| 1b6d7c2  | Establish INT SDD Line 9 Naming and Identifiers  | main   |
| 28de970  | SDD codebase refinement till Line 8              | main   |
| 7eb4779  | Upto step 6 SDD Codebase Setup                   | main   |

Push status: Commit 71d3e1a is local. User held push pending their instruction.

---

## 3. Current Repository State

```
Repository root: d:\SDD_PROJECT_APPROACH_BY_TAPAS
Branch: main
Governance bootstrap: COMPLETE (Lines 1-11)
Real feature work: NOT STARTED
Business requirements: NOT SUPPLIED
Technology stack: TBD
Application architecture: TBD
CI/CD pipeline: NOT PRESENT
Security tooling (SAST/DAST): NOT INSTALLED (DEFERRED)
```

The repository is a technology-neutral SDD governance foundation. No application code, no real feature specs, no production tests, no CI/CD. This is correct at bootstrap completion.

Directory structure (confirmed):
```
d:\SDD_PROJECT_APPROACH_BY_TAPAS\
  README.md                    (11,651 bytes)
  .agent\rules\                (11 files)
  .agent\workflows\            (16 files)
  .ai-context\
    BRD.md                     (empty placeholder)
    architecture.md            (all TBD)
    constitution.md            (21,495 bytes — project-wide law)
    lifecycle.md               (8,655 bytes — state machine)
    project_context.md         (all UNDEFINED)
    prompt_history.md          (36,070 bytes — audit trail)
    slug-registry.md           (no active slugs)
    status.md                  (5,588 bytes)
    decisions\_template.adr.md (template only)
    plans\_template.plan.md    (template only)
    specs\_template.spec.md    (template only)
    tasks\_template.tasks.md   (template only)
    test_cases\_template.test_cases.md (template only)
    test_cases\_integration.md (initialized; no scenarios)
```

---

## 4. Working Lines vs Blueprint Section Numbers (CRITICAL)

The user defined an 11-Line roadmap. Lines 1-11 are INTERNAL to the user's sequence and do NOT align 1:1 with Blueprint section numbers. The roadmap ends at Line 11. There is NO Working Line 12.

| Line | Line Name               | Blueprint Section(s)          |
|------|-------------------------|-------------------------------|
| 1    | Engineering Approach    | Section 1 — Philosophy        |
| 2    | AI Usage                | Section 2 — AI operating model|
| 3    | Core SDD Rule           | Section 3 — SDD methodology   |
| 4    | Non-Negotiable Rules    | Section 4 — Hard constraints  |
| 5    | Required Lifecycle      | Section 5 — Lifecycle model   |
| 6    | Specification Standard  | Section 6 — One feature, one spec |
| 7    | Gate 1                  | Section 12 — Spec peer review |
| 8    | Architecture/Repository | Sections 8-11 — Architecture  |
| 9    | Naming / Prompting Rule | Section 7 — Naming/identifiers|
| 10   | Development Rule        | Section 13 — Development disc.|
| 11   | QA & Security           | Sections 14-15 — QA/security  |

KEY EXAMPLE: Blueprint Section 12 = Gate 1 = Working Line 7. Same concept, different numbers. Do not create Working Line 12 from Blueprint Section numbering.

---

## 5. Official 11-Line Roadmap — Core Principles

Line 1 — Engineering Approach: Spec is primary artefact; code is generated output. No agent output ships without a named human accountable. Speed comes from clarity, not from skipping steps.

Line 2 — AI Usage: AI-first does not mean AI does the thinking. Agents are literal-minded, context-dependent. Context lives in repository, not chat. Model choice is a tool decision.

Line 3 — Core SDD Rule: Structured, version-controlled spec is primary artefact. Chain: BRD to Spec to Plan to Tasks to Test-first to Implementation.

Line 4 — Non-Negotiable Rules: Six hard gates (none waiveable): (1) approved spec before implementation, (2) no spec-less prompting, (3) test-first with RED, (4) no secrets/PII in artefacts, (5) scoped context only, (6) human accountability for every merged line.

Line 5 — Required Lifecycle: Discovery to Constitution to Spec to Gate 1 to Plan to Architecture Check to Tasks to Test-first (RED) to Implementation (GREEN) to Gate 2 to Merge to Release to Production Support to Feedback.

Line 6 — Specification Standard: One feature, one spec. Intent and ACs before technical detail. ACs must be testable (Given/When/Then) with stable IDs. API contract mandatory when API surface exists.

Line 7 — Gate 1: Pre-code design/intent validation. Named human reviewer, non-author. AI cannot approve. Seven dimensions: ambiguity, testability, scope, API contract, constitution compliance, overlap, dependencies.

Line 8 — Architecture & Repository: Every plan checked against constitution.md before task generation. Significant architectural decisions require ADR. One feature branch per spec, same slug throughout.

Line 9 — Naming & Identifiers: Slug is governance infrastructure, not cosmetic. Prompt by identity: feature.T03, feature.AC3, feature.API02. Slugs: kebab-case, 3-5 words, verb-free, unique for project lifetime.

Line 10 — Development Rule: One task, one prompt. Cycle: generate to review to commit. When significantly wrong: stop, classify failure source (task/plan/spec/ADR), fix authoritative artefact, regenerate.

Line 11 — QA & Security: Tests derive from ACs, not finished code. AI-generated tests are a starting point. Security applies regardless of code authorship. SAST/DAST mandatory; tooling deferred until pipeline established.

---

## 6. Line-by-Line Implementation Status (All Confirmed)

| Line | Name              | Files                                                | Audit Status |
|------|-------------------|------------------------------------------------------|--------------|
| 1    | Engineering Appr. | Philosophy in constitution.md; non-negotiables.md    | CONFIRMED    |
| 2    | AI Usage          | AI model in constitution.md; auto-log.md; .agentignore | CONFIRMED  |
| 3    | Core SDD Rule     | Artefact chain in constitution.md; lifecycle.md      | CONFIRMED    |
| 4    | Non-Negotiables   | int-sdd-non-negotiables.md; pre-implementation-compliance.md | CONFIRMED |
| 5    | Lifecycle         | lifecycle.md (90 lines); sdd-lifecycle-check.md      | CONFIRMED    |
| 6    | Spec Standard     | int-sdd-specification-standard.md (18 sections)     | CONFIRMED    |
| 7    | Gate 1            | int-sdd-gate-1.md (531 lines); gate-1-review.md     | CONFIRMED    |
| 8    | Architecture      | int-sdd-architecture.md (511 lines); all workflows   | CONFIRMED    |
| 9    | Naming            | int-sdd-naming.md (14 sections); naming-identifier-check.md | CONFIRMED |
| 10   | Development       | int-sdd-development.md (648 lines); task-by-task-development.md | CONFIRMED |
| 11   | QA & Security     | int-sdd-qa.md (452 lines); int-sdd-security.md (473 lines) | CONFIRMED |

ALL 11 Lines confirmed. No claimed file was found absent.

Lines 1-3 have no standalone rule files by design; governance is embedded in constitution.md.

---

## 7. Complete File Inventory

### .agent/rules/ — Canonical Governance Rules

| File                                | Size      | Authority Level      | Lines |
|-------------------------------------|-----------|----------------------|-------|
| .agentignore                        | 712 B     | OPERATIONAL BOUNDARY | 2,4   |
| auto-log.md                         | 859 B     | RULE                 | 2     |
| int-sdd-architecture.md             | 15,916 B  | AUTHORITATIVE RULE   | 8     |
| int-sdd-development.md              | 23,426 B  | AUTHORITATIVE RULE   | 10    |
| int-sdd-gate-1.md                   | 19,237 B  | AUTHORITATIVE RULE   | 7     |
| int-sdd-naming.md                   | 22,590 B  | AUTHORITATIVE RULE   | 9     |
| int-sdd-non-negotiables.md          | 5,950 B   | AUTHORITATIVE RULE   | 4     |
| int-sdd-qa.md                       | 16,358 B  | AUTHORITATIVE RULE   | 11    |
| int-sdd-security.md                 | 17,548 B  | AUTHORITATIVE RULE   | 11    |
| int-sdd-specification-standard.md   | 13,159 B  | AUTHORITATIVE RULE   | 6     |
| int-standards.TBD.md                | 513 B     | PLACEHOLDER          | 8     |

### .agent/workflows/ — Operational Procedures

| File                             | Size      | Invoked When                          | Lines |
|----------------------------------|-----------|---------------------------------------|-------|
| adr-check.md                     | 6,925 B   | Determining ADR requirement           | 8     |
| architecture-check.md            | 9,963 B   | After plan; before task generation    | 8     |
| code-review.md                   | 9,014 B   | Step 8 of task-by-task-development    | 10,11 |
| development-discipline-check.md  | 14,537 B  | Validating task readiness/scope       | 10    |
| gate-1-review.md                 | 16,835 B  | During Gate 1 peer review             | 7     |
| generate-plan.md                 | 4,082 B   | Creating plan from approved spec      | 8     |
| generate-tests.md                | 4,584 B   | Creating test cases from approved spec| 6,11  |
| naming-identifier-check.md       | 13,024 B  | Before spec; Gate 1; Gate 2           | 9     |
| pre-implementation-compliance.md | 2,923 B   | Hard gate before implementation       | 4,10  |
| qa-validation-check.md           | 13,080 B  | QA validation (7 parts); Gate 2 QA   | 11    |
| repository-check.md              | 6,518 B   | Pre-branch; pre-PR; Gate 2 prep       | 8     |
| sdd-lifecycle-check.md           | 2,696 B   | Before any lifecycle-stage action     | 5     |
| security-check.md                | 17,511 B  | Before commit (Part 1); Gate 2 (Part 8)| 11  |
| spec-prewriting-gate.md          | 5,616 B   | Before drafting any spec              | 6     |
| spec-review.md                   | 11,688 B  | Before entering Gate 1                | 6,7   |
| task-by-task-development.md      | 16,150 B  | 12-step implementation loop           | 10    |

### .ai-context/ — Living Governance Documents

| File               | Size      | Type               | Current State                          |
|--------------------|-----------|--------------------|----------------------------------------|
| BRD.md             | 493 B     | LIVING DOCUMENT    | Empty; no business requirements        |
| architecture.md    | 1,568 B   | LIVING DOCUMENT    | Header only; all sections TBD          |
| constitution.md    | 21,495 B  | AUTHORITATIVE LAW  | Complete; all 11 Lines integrated      |
| lifecycle.md       | 8,655 B   | AUTHORITATIVE REF  | Complete state machine                 |
| project_context.md | 2,097 B   | LIVING DOCUMENT    | All fields UNDEFINED                   |
| prompt_history.md  | 36,070 B  | AUDIT TRAIL        | Append-only; Lines 6-11 recorded       |
| slug-registry.md   | 1,962 B   | REGISTRY           | No active slugs (correct at bootstrap) |
| status.md          | 5,588 B   | STATUS BOARD       | Bootstrap Complete; no feature active  |

### Templates (all contain only template content)

| File                          | Location               |
|-------------------------------|------------------------|
| _template.adr.md              | .ai-context/decisions/ |
| _template.spec.md             | .ai-context/specs/     |
| _template.plan.md             | .ai-context/plans/     |
| _template.tasks.md            | .ai-context/tasks/     |
| _template.test_cases.md       | .ai-context/test_cases/|
| _integration.md               | .ai-context/test_cases/ (initialized; no scenarios) |

Root: README.md (11,651 B) — navigation; Line summaries; links to rules and workflows.

---

## 8. Governance Authority Hierarchy

When two files discuss the same concept, canonical authority wins.

| Concern               | CANONICAL AUTHORITY            | OPERATIONAL WORKFLOW(S)         | NOT AUTHORITATIVE              |
|-----------------------|--------------------------------|---------------------------------|--------------------------------|
| Project-wide law      | constitution.md                | pre-implementation-compliance.md| Any rule conflicting with const|
| Spec quality          | int-sdd-specification-standard | spec-prewriting-gate, spec-review| gate-1-review (checks, not defines)|
| Gate 1                | int-sdd-gate-1.md              | gate-1-review.md                | Any AI review output           |
| Architecture          | int-sdd-architecture.md        | architecture-check, adr-check   | architecture.md (reports state)|
| ADR significance      | int-sdd-architecture.md §5-7   | adr-check.md                    | _template.adr.md (structure)   |
| Naming / slugs        | int-sdd-naming.md              | naming-identifier-check         | README.md (summarises)         |
| Development discipline| int-sdd-development.md         | task-by-task-development        | Generated task descriptions    |
| Test-first rule       | int-sdd-non-negotiables.md §3  | generate-tests; qa-validation Part 3 | Passing test output       |
| QA governance         | int-sdd-qa.md                  | qa-validation-check             | AI-generated test results      |
| Security governance   | int-sdd-security.md            | security-check; code-review §5  | Absence of scanner findings    |
| Non-negotiable gates  | int-sdd-non-negotiables.md     | pre-implementation-compliance   | Nothing overrides these gates  |
| Lifecycle states      | lifecycle.md                   | sdd-lifecycle-check             | status.md (reports, not defines)|
| Slug registry         | slug-registry.md               | naming-identifier-check         | Prior conversation slug refs   |
| AI attribution prohib | int-sdd-non-negotiables.md §6  | code-review; repository-check   | Agent-authored commits/PRs     |
| Context scoping       | int-sdd-non-negotiables.md §5  | pre-implementation-compliance   | Repository-wide dumps          |

Conflict resolution: Identify the inconsistency, determine which artefact owns the decision, update that artefact, then continue.

---

## 9. Rule to Workflow to Template Architecture

```
RULE                              WORKFLOW(S)                       TEMPLATE
int-sdd-specification-standard    spec-prewriting-gate.md           _template.spec.md
                                  spec-review.md
                                  generate-tests.md (partial)

int-sdd-gate-1.md                 gate-1-review.md                  (none)

int-sdd-architecture.md           architecture-check.md             _template.plan.md
                                  adr-check.md                      _template.adr.md
                                  repository-check.md
                                  generate-plan.md

int-sdd-naming.md                 naming-identifier-check.md        (slug-registry.md is registry)
                                  spec-prewriting-gate.md Q0

int-sdd-non-negotiables.md        pre-implementation-compliance.md  (none)

int-sdd-development.md            task-by-task-development.md       _template.tasks.md
                                  development-discipline-check.md
                                  code-review.md

int-sdd-qa.md                     qa-validation-check.md            _template.test_cases.md
                                  generate-tests.md                 _integration.md

int-sdd-security.md               security-check.md                 (checklist is in workflow)
                                  code-review.md §5

constitution.md                   (governs all of the above)
lifecycle.md                      sdd-lifecycle-check.md
```

---

## 10. Complete Artefact Chains

### Governance artefacts (exist now)

```
constitution.md (root authority)
    references: lifecycle.md (state machine)
    references: int-sdd-*.md rules (canonical per Line)
    consumed by: *.md workflows (operational procedures)
    operate on: templates (structural patterns)
```

### Feature delivery chain (per future feature)

```
BRD.md (BRD-NNN) -> slug assigned -> slug-registry.md
  -> specs/<slug>.spec.md  [Gate 1: human approval]
  -> plans/<slug>.plan.md  [Architecture Check]
  -> decisions/ADR-NNNN-*.md  [when significant decision required]
  -> tasks/<slug>.tasks.md  [Test-first: RED required before implementation]
  -> test_cases/<slug>.test_cases.md
  -> feature/<slug> branch  [generate-review-commit per task]
  -> PR  [Gate 2: qa-validation + security-check + human approval]
  -> merge to main (squash) -> Release -> Production Support -> Feedback
```

---

## 11. Traceability & Identifier Architecture

### Identifier Hierarchy

```
BRD-NNN                 (global project requirement)
    -> <feature-slug>   (canonical root; kebab-case; 3-5 words)
        <slug>.ACN      Acceptance criterion     (N=1,2,3 sequential)
        <slug>.APINN    API contract endpoint    (NN=01,02 zero-padded)
        <slug>.UTNN     Unit test case           (NN=01,02; DEFINED IN SPEC §11)
        <slug>.TNN      Task                     (NN=01,02 zero-padded)
        <slug>.TCNN     QA test case             (NN=01,02; DEFINED IN test_cases file)

ADR-NNNN    Architectural Decision Record (global sequential)
INT-NN      Integration scenario (in _integration.md)
```

### Stability Rules

Slug assigned once at spec creation; cannot change while downstream artefacts exist. Retired slug is permanently unavailable; never reused. All sub-identifiers are stable; never renumbered; retired IDs not reassigned. Slug registry is single source of truth for collision detection.

### Prompt-by-Identity Rule

CORRECT: Implement <slug>.T03. Satisfy <slug>.AC3. Follow <slug>.API02.
WRONG:   "Implement the login thing and make sure it works securely."

### Traceability Map

```
BRD-NNN -> <slug>
  -> <slug>.AC1  referenced in: <slug>.plan.md (approach) and <slug>.tasks.md (TNN column)
  -> <slug>.UT01 defined in: <slug>.spec.md §11; maps to: <slug>.test_cases.md (TC01)
  -> <slug>.TC01 defined in: <slug>.test_cases.md
  -> <slug>.T01  defined in: <slug>.tasks.md
  -> feature/<slug> branch
  -> commits with "<slug>.T01" in message
  -> PR -> Gate 2 -> merge -> release -> status.md
```

---

## 12. Lifecycle & State Machine

### Delivery stages (from lifecycle.md)

```
Discovery -> Constitution -> Spec -> Gate 1 -> Plan -> Architecture Check
-> Tasks -> Test-first (RED) -> Guided Implementation (GREEN)
-> Gate 2 -> Merge -> Release -> Production Support -> Feedback
```

No stage may be skipped or reordered.

### Feature specification states

Draft -> In Peer Review -> Changes Requested -> In Peer Review (loop)
-> Approved -> Plan Drafted -> Plan Reviewed -> Tasks Generated
-> In Development -> In QA -> Ready for Release -> Released (vX.Y.Z)
-> Deprecated / Superseded

### Task states

Not Started -> In Progress -> In Review -> Merged

- In Review = committed; Gate 2 PENDING. Does NOT mean merged.
- Merged = Gate 2 completed AND branch merged. Requires human Gate 2 approval.
- AI code generation does NOT equal In Review.

### State change authority

Draft->In Peer Review: spec author (human)
In Peer Review->Approved: named human Gate 1 reviewer (not author)
Approved->Plan Drafted: plan author after Gate 1 evidence
Tasks Generated->In Development: technical owner after architecture check
In QA->Ready for Release: after Gate 2 evidence complete

Agents may NOT self-approve Gate 1, declare Merged without Gate 2, or declare Released without evidence.

Hotfix: HOTFIX-<incident-slug>. Gate 1 may be deferred; Gate 2 is NEVER skipped.

---

## 13. Gate 1 vs Task Review vs Gate 2

Three DISTINCT events. Do not collapse them.

| Gate       | When                           | Purpose                                      | Who                    | Output                            |
|------------|--------------------------------|----------------------------------------------|------------------------|-----------------------------------|
| Gate 1     | After spec; before impl        | Pre-code design/intent validation            | Named human non-author | Approved/Changes Requested in spec|
| Task review| After generation; before commit| Verify changeset implements one task         | Human (AI-assisted)    | Reviewed diff; commit             |
| Gate 2     | After all tasks; before merge  | Diff review; ACs, RED/GREEN, security, arch  | Named human reviewer   | Merge authorization               |

Ordering: Gate 1 -> tasks -> task reviews -> Gate 2 -> merge.

code-review.md supports TASK-LEVEL review. Gate 2 evidence = qa-validation-check.md Part 7 + security-check.md Part 8 + repository-check.md.

---

## 14. AI Operating Model

Governing: constitution.md §AI Operating Model; int-sdd-non-negotiables.md; int-sdd-development.md; .agentignore; auto-log.md.

Agents MAY:
- Analyze spec ambiguity
- Generate implementation code
- Generate test starting points
- Assist with code review
- Update prompt_history.md
- Propose ADR drafts
- Flag spec gaps
- Classify failure source
- Produce security/QA check outputs

Agents may NOT:
- Self-approve Gate 1
- Declare code reviewed without human review
- Declare AI tests as QA closure
- Approve merge
- Mark status Merged without Gate 2
- Self-authorize a security exception
- Silently widen task scope
- Claim SAST passed when no scanner ran
- Create Working Line 12

Core rules: Context lives in repository not chat. Prompt by identity once identifiers exist. Load only scoped context (.agentignore). One task, one prompt. Log work to prompt_history.md. No AI attribution in commits, PRs, or code comments.

---

## 15. Development Discipline

Authoritative rule: int-sdd-development.md (648 lines, 16+ sections)
Operational: task-by-task-development.md (12 steps)
Pre-condition gate: pre-implementation-compliance.md

Core contract:
```
SELECT one approved task
-> VERIFY readiness (pre-implementation-compliance.md)
-> LOAD scoped context only
-> PROMPT by task identity
-> GENERATE change
-> INSPECT diff
-> VERIFY against task ID, AC IDs, tests, scope
-> REVIEW (code-review.md)
-> COMMIT (after review)
-> UPDATE status/traceability
-> PROCEED to next task
```

Implementation prompt structure:
```
Implement <slug>.TNN.
Satisfy <slug>.ACN.
Follow <slug>.APINN [if applicable].
Context (load only): [specific files]
Do not modify: [prior tasks, future tasks, deferred scope]
Expected verification: [test ID] passes GREEN
```

When generation is significantly wrong:
STOP re-prompting -> Classify defect source (Prompt/Task/Plan/Spec/ADR/Context/Implementation)
-> Fix the authoritative source -> Regenerate from corrected source.
Never patch code to compensate for an ambiguous spec.

Commit format: <type>(<slug>): <description>. No AI attribution. Use SDD identifiers: Implements <slug>.T03.

---

## 16. QA Architecture

Authoritative rule: int-sdd-qa.md (452 lines, 13 sections)
Workflows: qa-validation-check.md (7 parts); generate-tests.md
Templates: _template.test_cases.md; _integration.md

QA source-of-truth chain:
Approved Spec -> ACs -> Spec-derived test cases -> QA review
-> Test-first / RED (before implementation)
-> Implementation -> GREEN
-> QA judgement (is the test suite sufficient?)
-> UAT / regression where applicable

BLOCKING RULE: Tests MUST derive from the approved spec, not from reading finished implementation.

### TC vs UT Namespaces: SPECIAL AUDIT — VERIFIED

Both namespaces are intentionally distinct. No conflict found.

| Namespace | Format      | Defined in               | Purpose                                          |
|-----------|-------------|--------------------------|--------------------------------------------------|
| UT        | <slug>.UTNN | specs/<slug>.spec.md §11 | Spec-level unit-test; maps to executable functions|
| TC        | <slug>.TCNN | test_cases/<slug>.test_cases.md | QA test-case; includes negative/boundary/manual/UAT|

Sources confirming distinction: int-sdd-qa.md §3.2; _template.test_cases.md §Test Identity Reference; int-sdd-specification-standard.md §11.

Both are stable. Neither renumbered. Retired IDs never reassigned. TC references corresponding UT where one exists. A TC may exist without a UT (manual/UAT scenarios). A UT without a TC is valid.

AC-to-test traceability:
<slug>.AC1 -> <slug>.TC01 (in test_cases file)
           -> <slug>.UT01 (in spec §11)

AI-generated tests: starting point for QA review, NOT QA approval. GREEN proves implementation satisfies the executed test; it does NOT prove the suite fully captures the requirement. qa-validation-check.md Part 4 governs AI-test review.

QA failure escalation: A=test defect, B=implementation defect, C=task defect, D=plan defect, E=spec defect (fix through Gate 1), F=architecture/ADR defect, H=context/data issue.

---

## 17. Security Architecture

Authoritative rule: int-sdd-security.md (473 lines, 13 sections)
Operational workflow: security-check.md (8 parts)
Also in: code-review.md §5; constitution.md §Security Posture; int-sdd-non-negotiables.md §4

FOUNDATIONAL PRINCIPLE: The code's origin does not change the security standard. AI-generated code receives identical security controls.

Explicitly prohibited:
- "AI wrote it, so the scanner is optional." -> NON-COMPLIANT
- "Tests passed, so security is covered." -> NON-COMPLIANT

| Control                        | Governance                           | Status                           |
|--------------------------------|--------------------------------------|----------------------------------|
| Secrets/PII prohibition        | int-sdd-security.md §2               | IMPLEMENTED (rule enforceable)   |
| Dependency vetting             | int-sdd-security.md §3; Part 2       | IMPLEMENTED (rule); tooling DEFERRED|
| SAST                           | int-sdd-security.md §4; Part 3       | DOCUMENTED — DEFERRED until pipeline|
| DAST                           | int-sdd-security.md §4; Part 3       | DOCUMENTED — DEFERRED until pipeline|
| Dependency scanning            | int-sdd-security.md §4; Part 3       | DOCUMENTED — DEFERRED until pipeline|
| Injection (7 classes incl. prompt injection)| int-sdd-security.md §5; Part 4 | DOCUMENTED — no application yet|
| Authentication/Authorization   | int-sdd-security.md §6; Part 5       | DOCUMENTED — no implementation   |
| Least privilege                | int-sdd-security.md §7; Part 6       | DOCUMENTED — no infrastructure   |
| Data at rest/in transit        | int-sdd-security.md §8; Part 7       | DOCUMENTED — no datastore        |
| Security exceptions            | int-sdd-security.md §9               | DOCUMENTED — human sign-off req  |
| Gate 2 checklist (16 controls) | int-sdd-security.md §10; Part 8      | DOCUMENTED                       |
| Prompt injection / ext. content| int-sdd-security.md §11              | DOCUMENTED                       |

DEFERRED does NOT mean WAIVED. When pipeline is established, deferred controls become mandatory.

Security failure escalation mirrors QA: B=implementation, E=spec (fix via Gate 1), G=governance. Do not patch code when root cause is a missing security requirement in the spec.

---

## 18. Repository & Git Governance

Authoritative rule: int-sdd-architecture.md Part B; repository-check.md

Feature branches: feature/<feature-slug> (one per approved spec)
Hotfix branches: hotfix/<incident-slug>
Merge: Squash merge into main/trunk
Commit format: <type>(<slug>): <description>
AI attribution: PROHIBITED in commits, PRs, code comments — use SDD identifiers
Secrets/PII: Never in commit content
Gate 2: Must complete before merge

---

## 19. Status & Progress Management

File: .ai-context/status.md — governed by lifecycle.md §Status Board Currency

Update on the same day as each lifecycle state transition. Required fields per feature: Spec ID, title, status, owner, last updated, notes/blockers.

Current confirmed state: Bootstrap Complete (Lines 1-11). No feature spec approved. No feature work in progress.

---

## 20. Prompt History / AI Audit Trail

File: .ai-context/prompt_history.md (36,070 bytes, append-only)
Governing rule: auto-log.md

Sessions recorded (confirmed):
- Line 11 QA & Security Governance (2026-10-04)
- Line 10 Development Discipline (2026-09-30)
- Line 9 Naming and Identifier Governance (2026-09-29)
- Lines 7-8 Architecture / Gate 1 (2026-09-27)
- Line 6 Specification Standard (2026-09-27)
- Lines 1-5 bootstrap sessions (2026-09-25)

Safety rule: No secrets, credentials, tokens, PII, or confidential data in history entries.

---

## 21. Architecture / ADR Governance

Authoritative rule: int-sdd-architecture.md §5-7
Assessment workflow: adr-check.md
ADR store: .ai-context/decisions/
Template: _template.adr.md

ADR is required for: new system structure, new data persistence, new external integration, new authentication mechanism, new significant infrastructure choice, technology choice constraining future architecture, or decision contradicting existing ADR.

Current state: NO ADRS EXIST. Only _template.adr.md is present. Correct at bootstrap.
ADR format: ADR-NNNN-<decision-slug>.md (e.g., ADR-0001-authentication-mechanism.md)
architecture.md: governance header only; all sections TBD. Correct — no architecture designed.

---

## 22. File-by-File Relationship Map

constitution.md: Root authority. All files subordinate. Updated through Line 11. Contains: SDD artefact model; non-negotiable gates; AI operating model; testing discipline; security posture; architectural constraints.

lifecycle.md: Authoritative state machine. Consumed by sdd-lifecycle-check.md and all gate documentation. Contains: lifecycle stages; spec states; task states; gates; hotfix; status board currency; feedback loop.

int-sdd-non-negotiables.md: Six hard enforcement gates. Cannot be waived. Consumed by pre-implementation-compliance.md. Critical: Never execute Prompt to Code, Idea to Code, or Unapproved spec to Code.

int-sdd-specification-standard.md: Authoritative spec quality standard. 18 sections. Consumed by spec-prewriting-gate.md, spec-review.md, _template.spec.md. Depends on constitution.md and int-sdd-naming.md §2.

int-sdd-gate-1.md: Canonical Gate 1 governance. Consumed by gate-1-review.md. Defines: what Gate 1 is/is not; reviewer requirements (named, non-author, human); seven dimensions; AI reviewer prohibition.

int-sdd-architecture.md: Architecture governance and repository standards. Consumed by architecture-check.md, adr-check.md, repository-check.md, generate-plan.md. Defines: plan-to-constitution check; ADR significance; branch naming; squash-merge; AI attribution prohibition.

int-sdd-naming.md: Feature slug as canonical root identity. 14 sections. Consumed by naming-identifier-check.md, spec-prewriting-gate.md Q0. Defines: slug format; prohibited patterns; slug lifecycle; rename/retire/archive; prompt-by-identity.

int-sdd-development.md: Authoritative development discipline. 16+ sections. Consumed by task-by-task-development.md, development-discipline-check.md, code-review.md. Defines: core contract; one-task-one-prompt; prompt structure; generate-review-commit; failure source classification; commit governance.

int-sdd-qa.md: Authoritative QA governance. 13 sections. Consumed by qa-validation-check.md, generate-tests.md, _template.test_cases.md. Defines: QA source-of-truth chain; AC-to-test derivation (blocking); TC/UT namespace; AI-test governance; UAT spec-gap protocol; failure escalation (A-H).

int-sdd-security.md: Authoritative security governance. 13 sections. Consumed by security-check.md, code-review.md §5. Defines: authorship independence; secrets/PII; dependency vetting; SAST/DAST (deferred-not-waived); injection (7 classes); auth/authz; least privilege; data protection; Gate 2 checklist (16 controls).

pre-implementation-compliance.md: Hard gate before every implementation code change. 10-item checklist. Consult before every task start. Depends on int-sdd-non-negotiables.md and int-sdd-development.md.

code-review.md: Task-scoped pre-commit review. Used at Step 8 of task-by-task-development.md. Reviews: task identity; AC verification; test evidence; API contract; security (14-item list); scope compliance; commit decision.

---

## 23. Rule-to-File Matrix

| Governance Rule          | Canonical Authority                  | Operational Workflow                    | Template                 |
|--------------------------|--------------------------------------|-----------------------------------------|--------------------------|
| Spec-first               | constitution.md                      | spec-prewriting-gate.md                 | _template.spec.md        |
| Approved spec required   | int-sdd-non-negotiables.md §1        | pre-implementation-compliance.md        | —                        |
| Gate 1                   | int-sdd-gate-1.md                    | gate-1-review.md                        | —                        |
| Architecture check       | int-sdd-architecture.md §1           | architecture-check.md                   | _template.plan.md        |
| ADR significance         | int-sdd-architecture.md §5-7         | adr-check.md                            | _template.adr.md         |
| Naming / slug            | int-sdd-naming.md                    | naming-identifier-check.md              | —                        |
| Stable IDs               | int-sdd-naming.md §3-4               | naming-identifier-check.md              | —                        |
| Prompt by identity       | int-sdd-naming.md §14; development §9| Task prompts                            | _template.tasks.md §Prompt|
| One task, one prompt     | int-sdd-development.md §2            | task-by-task-development.md             | _template.tasks.md       |
| Generate-review-commit   | int-sdd-development.md §3-6          | task-by-task-development.md; code-review| —                        |
| Test-first               | int-sdd-non-negotiables.md §3        | generate-tests.md; qa-validation Part 3 | _template.tasks.md §Test |
| AC-to-test traceability  | int-sdd-qa.md §3                     | qa-validation-check.md Parts 2-3        | _template.test_cases.md  |
| AI-test limitation       | int-sdd-qa.md §5                     | qa-validation-check.md Part 4           | test_cases §AI-Review    |
| UAT traceability         | int-sdd-qa.md §7                     | qa-validation-check.md Part 5           | test_cases §UAT          |
| Security scanning        | int-sdd-security.md §4               | security-check.md Part 3                | —                        |
| Dependency vetting       | int-sdd-security.md §3               | security-check.md Part 2                | —                        |
| Injection                | int-sdd-security.md §5               | security-check.md Part 4                | —                        |
| Auth/Authz               | int-sdd-security.md §6               | security-check.md Part 5                | —                        |
| Least privilege          | int-sdd-security.md §7               | security-check.md Part 6                | —                        |
| Data protection          | int-sdd-security.md §8               | security-check.md Part 7                | —                        |
| Gate 2                   | lifecycle.md §Gate 2                 | qa-validation Part 7; security Part 8   | —                        |
| Human accountability     | int-sdd-non-negotiables.md §6        | pre-implementation-compliance.md §Merge | —                        |
| AI attribution prohibit. | int-sdd-non-negotiables.md §6        | code-review.md; repository-check.md     | —                        |
| Secrets/PII              | int-sdd-non-negotiables.md §4        | security-check.md Part 1                | —                        |
| Context scoping          | int-sdd-non-negotiables.md §5        | pre-implementation-compliance.md        | .agentignore             |
| Lifecycle states         | lifecycle.md                         | sdd-lifecycle-check.md                  | —                        |
| Status board currency    | lifecycle.md §Status Board           | (manual update)                         | —                        |
| Spec change propagation  | int-sdd-specification-standard §14   | spec-review.md                          | —                        |

---

## 24. Cross-Line Dependency Map

Lines 1-3: Foundation for all Lines; embedded in constitution.md.

Line 4 (Non-Negotiables): Feeds Lines 5,6,7,9,10,11 with gate requirements. Secrets/PII rule strengthens Line 11.

Line 5 (Lifecycle): State machine consumed by all Lines. Gate 2 referenced by Lines 10 and 11.

Line 6 (Spec Standard): ACs -> Line 11 (test derivation source). API contract -> Line 10 (traceability). Slug -> Line 9.

Line 7 (Gate 1): Must complete before Lines 8,9,10,11 activate on any feature.

Line 8 (Architecture): Plan-to-constitution check -> Line 10. ADRs -> Lines 10,11. Branch naming -> Line 10.

Line 9 (Naming): Slug -> all Lines. Sub-identifiers (AC,API,UT,T,TC) -> Lines 10,11. Prompt-by-identity -> Line 10.

Line 10 (Development): Requires Lines 6,7,8,9. code-review §5 -> Line 11 security check. Commits -> Gate 2 (Line 5) requiring Line 11 evidence.

Line 11 (QA & Security): Requires Lines 6 (ACs), 9 (TC/UT IDs), 10 (task boundaries). QA + Security evidence -> Gate 2 (Line 5).

---

## 25. Confirmed Historical Changes

| Commit   | Line(s) | Key Files Added                                                                    |
|----------|---------|------------------------------------------------------------------------------------|
| 7eb4779  | 1-6     | .agentignore, auto-log.md, int-sdd-non-negotiables.md, int-sdd-specification-standard.md, lifecycle.md, constitution.md v1, all initial templates |
| 28de970  | 7-8     | int-sdd-architecture.md, int-sdd-gate-1.md, adr-check.md, architecture-check.md, gate-1-review.md, repository-check.md, _template.adr.md |
| 1b6d7c2  | 9       | int-sdd-naming.md, naming-identifier-check.md, slug-registry.md, _template.test_cases.md v1 |
| 5e02e79  | 10      | int-sdd-development.md, development-discipline-check.md, task-by-task-development.md |
| 71d3e1a  | 11      | int-sdd-qa.md, int-sdd-security.md, qa-validation-check.md, security-check.md, _integration.md |

All commit IDs confirmed against actual git log output.

---

## 26. Previous Completion Report Verification

| Line | Audit Status | Notes                                               |
|------|--------------|-----------------------------------------------------|
| 1    | CONFIRMED    | No standalone file by design; embedded in constitution |
| 2    | CONFIRMED    | No standalone file by design; AI model in constitution |
| 3    | CONFIRMED    | No standalone file by design; artefact chain in constitution |
| 4    | CONFIRMED    | int-sdd-non-negotiables.md present and verified     |
| 5    | CONFIRMED    | lifecycle.md full state machine present             |
| 6    | CONFIRMED    | int-sdd-specification-standard.md 18 sections present|
| 7    | CONFIRMED    | int-sdd-gate-1.md and gate-1-review.md present      |
| 8    | CONFIRMED    | int-sdd-architecture.md and all workflows present   |
| 9    | CONFIRMED    | int-sdd-naming.md 14 sections; naming-identifier-check.md present|
| 10   | CONFIRMED    | int-sdd-development.md 648 lines; all workflows present|
| 11   | CONFIRMED    | int-sdd-qa.md and int-sdd-security.md present; TC/UT distinction verified|

No discrepancy found between prior completion reports and actual repository contents.

---

## 27. Gaps / Risks / Ambiguities

GAP 1 [MEDIUM]: No standalone gate-2-review.md assembler workflow.
Gate 2 evidence is distributed across qa-validation-check.md Part 7, security-check.md Part 8, and repository-check.md. No single file assembles all Gate 2 requirements as gate-1-review.md does for Gate 1.
Impact: Functional but Gate 2 evidence may be assembled incompletely.
Action: Create gate-2-review.md before the first feature reaches Gate 2.

GAP 2 [INFORMATIONAL]: Lines 1-3 have no standalone rule files.
Design decision (embedded in constitution.md). Not a defect. A new engineer expecting one file per Line could be confused.

GAP 3 [INFORMATIONAL — correct at bootstrap]: architecture.md is entirely TBD.
Will need to be populated when first plan triggers architecture-check.md.

GAP 4 [LOW]: int-standards.TBD.md has non-standard filename pattern.
The .TBD. in the middle is unusual. The file itself instructs replacement with int-standards.<stack>.md. Resolves when stack is chosen.

GAP 5 [LOW]: sdd-lifecycle-check.md not explicitly enumerated in constitution.md governance items.
Cosmetic. Workflow is discoverable via project_context.md and lifecycle.md.

GAP 6 [INFORMATIONAL]: project_context.md not enumerated in constitution.md governance items.
Cosmetic. File is in standard location.

GAP 7 [LOW — CONFUSION RISK]: TC namespace formally introduced in Line 11 but implied from Line 6.
No governance conflict. TC/UT distinction is VERIFIED correct. A cross-reference from int-sdd-specification-standard.md §11 to int-sdd-qa.md §3.2 would eliminate potential confusion.

GAP 8 [LOW]: generate-plan.md is thin (4,082 bytes) compared to architecture-check.md (9,963 bytes).
May not comprehensively trigger all required checks. Review when first plan is generated.

GAP 9 [INFORMATIONAL — correct at bootstrap]: Named Gate reviewers not assigned.
Required by int-sdd-gate-1.md §3. Cannot assign until project team is established. BLOCKING for real feature delivery; not blocking for governance bootstrap.

---

## 28. Deferred / Future Project-Specific Items

| Item                      | Reason Deferred       | Blocking? | Future Trigger                   | Current Governance                  |
|---------------------------|-----------------------|-----------|----------------------------------|-------------------------------------|
| Business requirements     | No project defined    | No        | When project defined             | BRD.md placeholder                  |
| Feature specifications    | No BRD                | No        | After BRD-NNN approved           | Template exists                      |
| Technology stack          | No project            | No        | When BRD approved                | int-standards.TBD.md placeholder    |
| Application architecture  | Stack TBD             | No        | After stack approved             | architecture.md header              |
| ADRs                      | No decisions          | No        | When first plan triggers arch-check| Template + adr-check.md          |
| SAST tooling              | Pipeline TBD          | No (deferred not waived) | When pipeline established | int-sdd-security.md §4        |
| DAST tooling              | No interface          | No (deferred not waived) | When deployable interface | int-sdd-security.md §4        |
| Dependency scanning       | No dependencies       | No (deferred not waived) | When first dependencies   | int-sdd-security.md §4        |
| CI/CD pipeline            | No project            | No        | When stack/deployment approved   | Not yet designed                    |
| Cloud IAM roles           | No cloud provider     | No        | When infrastructure approved     | Least-privilege principle defined   |
| Named Gate reviewers      | No team               | YES — blocks real feature delivery | When team established | int-sdd-gate-1.md §3     |
| gate-2-review.md          | Gap — not yet created | No (evidence in 3 files) | Before first Gate 2       | qa-validation Part 7 + security Part 8|

---

## 29. Intentionally Absent Items

Real feature specs, BRD requirements, production code, frontend, backend, database, authentication implementation, real test suite, real integration tests, CI/CD pipeline, security scanner installation, cloud IAM roles, any ADR, real security findings, named Gate reviewers, gate-2-review.md (recommended future addition).

All absent items are correct at bootstrap. Nothing may be created without going through the full SDD artefact chain.

---

## 30. Repository Maturity

| Dimension                       | Status               |
|---------------------------------|----------------------|
| Governance foundation (Lines 1-11) | ESTABLISHED       |
| Artefact chain templates        | ESTABLISHED          |
| Lifecycle state machine         | ESTABLISHED          |
| Rule authority hierarchy        | ESTABLISHED          |
| Traceability model              | ESTABLISHED          |
| QA governance                   | ESTABLISHED          |
| Security governance             | ESTABLISHED          |
| Real feature delivery           | NOT STARTED          |
| Business requirements           | NOT SUPPLIED         |
| Application architecture        | TBD                  |
| Technology stack                | TBD                  |
| Security tooling (SAST/DAST)    | DEFERRED             |
| QA execution                    | NOT STARTED          |
| CI/CD pipeline                  | NOT PRESENT          |
| Human team assigned             | NOT CONFIRMED        |

---

## 31. Future Feature Execution Flow

LABEL: FUTURE OPERATIONAL MODEL. Not current execution.

STEP 1 DISCOVERY: Human captures business problem in BRD.md as BRD-NNN.

STEP 2 SLUG: Assign <feature-slug> per int-sdd-naming.md §2. Register in slug-registry.md. Confirm uniqueness.

STEP 3 SPEC: Copy _template.spec.md -> specs/<feature-slug>.spec.md. Complete: Intent, ACs (Given/When/Then with IDs), API Contract, UT IDs, out-of-scope, non-functional constraints. Run spec-review.md before Gate 1.

STEP 4 GATE 1: Named human reviewer, different from spec author. Seven dimensions reviewed. Record Approved or Changes Requested in spec Gate Approvals table. AI cannot approve Gate 1.

STEP 5 PLAN: Copy _template.plan.md -> plans/<feature-slug>.plan.md. Technical approach derived from approved spec. Explicitly address integration points, data model changes, deferred items. Run adr-check.md for each architectural decision.

STEP 6 ARCHITECTURE CHECK: Compare plan with constitution, architecture.md, ADRs. Create ADRs where required. Update architecture.md. PASS required before task generation.

STEP 7 TASKS: Copy _template.tasks.md -> tasks/<feature-slug>.tasks.md. Create feature branch: feature/<feature-slug>. Ordered tasks with AC/API traceability.

STEP 8 TEST-FIRST: For each task ACs: create test_cases/<feature-slug>.test_cases.md. TC IDs: <slug>.TCNN (in test_cases file). UT IDs: <slug>.UTNN (in spec §11). Run executable test -> observe FAIL (RED). Record RED evidence. Do NOT implement before RED confirmed.

STEP 9 IMPLEMENTATION (per task): Verify readiness (pre-implementation-compliance.md). Load scoped context only. Prompt: "Implement <slug>.TNN. Satisfy <slug>.ACN." Generate -> Inspect diff -> Review (code-review.md) -> Confirm GREEN. Commit: "<type>(<slug>): <description> — Implements <slug>.TNN". Update task state and status.md.

STEP 10 QA VALIDATION: qa-validation-check.md Parts 1-7. Validate AC coverage. Verify RED-to-GREEN. Review AI-generated tests. Verify UAT traceability.

STEP 11 SECURITY VERIFICATION: security-check.md Parts 1-8. All 16 Gate 2 security controls: Passed or N/A with justification. Deferred controls: documented as deferred (not Passed). Security exceptions: documented, human-signed.

STEP 12 GATE 2: Named human reviewer reviews actual diff. Confirms: ACs, RED/GREEN, security, architecture/docs. AI cannot approve Gate 2.

STEP 13 MERGE -> RELEASE -> FEEDBACK: Squash merge. Update spec to Released (vX.Y.Z). Update status.md. Production findings -> BRD.md -> new SDD cycle.

---

## 32. Instructions for the Next AI Agent

READ THIS SECTION BEFORE TOUCHING ANYTHING IN THIS REPOSITORY.

### Priority reading sequence

1. Read this document first.
2. Read .ai-context/constitution.md (project-wide law).
3. Read the canonical rule file for your current task (see §8 authority hierarchy).
4. Read the operational workflow for your specific action.
5. Read .ai-context/status.md for current project state.
6. Check .ai-context/slug-registry.md before assigning any slug.

### Behavioural rules

1. Do not trust chat memory over repository files. Claims in conversation not backed by artefacts are not authoritative.
2. Do not invent business requirements. BRD.md is empty. No requirements exist.
3. Do not create a real feature without the full SDD chain. BRD-NNN -> spec -> Gate 1 -> plan -> architecture check -> tasks -> test-first -> implementation. No shortcuts.
4. Do not bypass Gate 1. Requires a named human who is not the spec author.
5. Do not bypass test-first. RED must be confirmed before implementation.
6. Do not bypass Gate 2. Passing tests do not authorize merge.
7. Do not create Working Line 12 unless the user explicitly defines a new Line 12 task.
8. Prompt by identity once identifiers exist: Implement <slug>.TNN. Satisfy <slug>.ACN.
9. Security applies regardless of code authorship. Do not claim SAST passed when no scanner has run.
10. AI-generated tests are a starting point, not QA approval. QA judgement remains human.
11. DEFERRED means tooling pending, not waived.
12. Blueprint Section 12 = Gate 1 = Working Line 7. Do not confuse numbering.
13. TC (test_cases file) and UT (spec file) are DISTINCT namespaces. Both stable and intentional.
14. Authority hierarchy: constitution.md > canonical rule file > operational workflow > template.
15. Report ambiguity; do not infer. Use: AMBIGUITY, SOURCE, IMPACT, REQUIRED DECISION.

### When a future task references a Line

1. Read §4 to confirm the Line name and scope.
2. Read §6 for implementation status and which files implement it.
3. Read the canonical rule file in §8.
4. Use repository files, not conversation memory.

### If an expected file is not found

Do not invent contents. Report: "Expected file <path> was not found. This document listed it as <status>. Please verify."

### If a conflict is found

If a conflict exists between this document and actual repository contents, the repository files take precedence. This document reflects commit 71d3e1a. Post-audit commits are more recent.

---

## 33. Final Audit Verdict

| Check                                         | Verdict   |
|-----------------------------------------------|-----------|
| All 11 Working Lines implemented              | CONFIRMED |
| All claimed files exist with described content| CONFIRMED |
| No fake tests, features, or security findings | CONFIRMED |
| No application code                           | CONFIRMED |
| No invented business requirements             | CONFIRMED |
| TC/UT distinction correctly documented        | CONFIRMED — VERIFIED |
| Gate 1 vs Task Review vs Gate 2 distinct      | CONFIRMED |
| Working Line 7 = Blueprint Gate 1 / Section 12| CONFIRMED |
| Working Lines do not exceed 11                | CONFIRMED |
| Commit IDs match actual git log               | CONFIRMED |
| No discrepancy: prior reports vs actual files | CONFIRMED |

Gaps found: 9 (1 MEDIUM, 4 LOW, 4 INFORMATIONAL). No CRITICAL gaps. No BLOCKING governance defects.

SAFE TO USE AS MASTER HANDOVER CONTEXT: YES.

This document accurately represents the repository at commit 71d3e1a. All file paths, content descriptions, governance rules, and authority assignments were verified against actual repository contents. Historical claims are based on git commit evidence. No claims are made from conversation memory alone.

Document created: 2026-10-12. Commit: 71d3e1a, branch main.
Do not modify this document to fix governance gaps — fix the governance files and note changes on next audit.
