# Pre-Implementation SDD Compliance Check

## Purpose

Hard gate for every normal application-code change. Execute this check before creating or modifying implementation code. If any required answer is missing, conflicting, or unverified, stop; do not implement.

## Required Identification

1. **Feature slug:** `<feature-slug>`
2. **Authorizing spec:** `.ai-context/specs/<feature-slug>.spec.md`
3. **Gate 1 evidence:** status `Approved`, named human peer reviewer, reviewer differs from author, and approval recorded in repository.
4. **Derived plan:** `.ai-context/plans/<feature-slug>.plan.md`
5. **Task:** `<feature-slug>.TNN` in `.ai-context/tasks/<feature-slug>.tasks.md`
6. **Traceability:** relevant `<feature-slug>.ACN` and `<feature-slug>.APINN` IDs.
7. **Tests:** test-case documentation and executable test locations; test-to-AC mapping.
8. **RED evidence:** command, result, and reason the relevant test failed before implementation.
9. **Scoped context:** constitution sections, architecture sections, ADRs, source/tests in scope, and explicitly out-of-scope files.
10. **Safety:** secrets/PII screen and applicable security constraints.

## Decision

- **Proceed:** only when every required item is answered, consistent, and verified.
- **Stop:** on a missing spec, approval, plan, task, AC/API mapping, reviewed test, confirmed RED, scoped context, security condition, or conflict.
- **Required block message for missing approval chain:** `Implementation blocked: approved SDD specification is required.` Identify the exact missing artefact or approval.
- **Ambiguity report:** `AMBIGUITY`, `SOURCE`, `IMPACT`, `REQUIRED DECISION`.
- **Broad task report:** `Task scope is too broad for a bounded SDD execution increment.`

## Post-Implementation Check

Before declaring a task complete, verify:

- Task behaviour matches the approved spec and each relevant AC is individually verified.
- Relevant tests progressed from documented RED to executed GREEN.
- No unrelated files or unapproved scope were introduced.
- Security constraints were checked; no secrets or PII entered artefacts, logs, prompts, or commits.
- Architecture, ADR, test-case documentation, status, and prompt history are updated when applicable.
- Human Gate 2 review remains pending or is recorded; passing tests alone do not authorize merge.

## Merge Check

Codex may assist but cannot approve merge. Require evidence of approved-task traceability, AC verification, RED-to-GREEN results, security checks, controlled scope, current documentation, and completed human Gate 2 review. Do not use AI attribution as evidence or in merge metadata.
