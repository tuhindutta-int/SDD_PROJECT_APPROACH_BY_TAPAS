# INT SDD Non-Negotiable Enforcement

## Authority

This is the authoritative enforcement rule for normal implementation work. It supplements `.ai-context/constitution.md`; if a requirement cannot be satisfied, the agent must stop and report the blocking condition. Convenience, urgency, apparent simplicity, local success, or AI confidence do not waive these gates.

## 1. Approved SDD Chain Required

Before changing application code, verify all of the following:

- A valid `<feature-slug>` spec exists at `.ai-context/specs/<feature-slug>.spec.md`.
- The spec is explicitly `Approved` through the repository Gate 1 process.
- Gate 1 records a named human peer reviewer who is not the spec author and records that person's approval.
- An approved plan exists at `.ai-context/plans/<feature-slug>.plan.md`.
- An approved task set exists at `.ai-context/tasks/<feature-slug>.tasks.md`.
- The requested `<feature-slug>.TNN` is in that task set and maps to its plan, spec, and relevant AC/API IDs.

Never execute `Prompt → Code`, `Idea → Code`, `Chat message → Code`, or `Unapproved spec → Code`. If any condition fails, stop and report exactly: **Implementation blocked: approved SDD specification is required.** Then name the missing artefact or approval.

## 2. No Spec-Less Prompting

Vague instructions or code-focused requests do not authorize implementation. Identify the missing BRD, spec, plan, task, acceptance criterion, API contract, or decision; record or request it through the governing artefact and required gate. The agent must not turn ambiguous intent into production behaviour.

## 3. Test-First, Confirmed RED

The mandatory sequence is: `Spec → AC → test-case documentation → reviewed executable test → executed RED → implementation → executed GREEN`.

- Every specification-derived test identifies its source `<feature-slug>.ACN`.
- Test-first evidence requires the relevant executable test to be run and observed failing before implementation.
- Do not implement first, retrofit tests, weaken tests, change expected outcomes merely to pass, or claim RED without execution evidence.
- If a new test passes before implementation, stop and report why RED did not occur; investigate existing behaviour, test correctness, test coverage, or pre-existing implementation before proceeding.

## 4. No Secrets or PII

Before writing an SDD artefact, prompt history entry, agent instruction, commit message, or AI prompt, screen content for secrets and PII. Do not persist passwords, API keys, access tokens, private credentials, production secrets, customer personal data, payment data, government identifiers, or confidential authentication information.

Redact sensitive content completely or replace it with a synthetic placeholder such as `<synthetic-email>` or `<REDACTED>`, and report that exclusion when material. Partial masking is not assumed safe. Never commit credential-bearing `.env` files.

## 5. Scoped Context Only

Use `.agent/rules/.agentignore` as a first-level boundary. For each task, load only the constitution, approved spec, approved plan, task, applicable AC/API contract, relevant architecture section, directly related source/tests, and applicable ADRs. State why repository-wide analysis is necessary before performing it. Do not ingest unrelated modules, histories, generated output, binaries, secrets, or documentation.

## 6. Human Accountability and Merge

Every merged line remains the responsibility of a human engineer. AI authorship, AI-generated tests, or passing tests do not constitute approval. Before merge, a human must be able to explain the change, its task ID, satisfaction of each AC, test evidence, security considerations, architecture/documentation impact, and intentional exclusions.

Codex must not use AI-attribution text in source comments, commit messages, PR descriptions, or implementation notes. Use stable SDD traceability instead, such as `Implements <feature-slug>.T03`.

Merge requires human Gate 2 evidence that approved-task traceability, AC verification, RED-to-GREEN evidence, security checks, controlled scope, current documentation, and review are complete.

## Scope, Ambiguity, and Consistency Gates

- Execute one bounded task at a time. If it is too broad, stop and report: **Task scope is too broad for a bounded SDD execution increment.**
- Do not introduce undeclared features, refactors, business rules, APIs, data stores, architecture changes, dependencies, or behaviours.
- For ambiguity, report **AMBIGUITY**, **SOURCE**, **IMPACT**, and **REQUIRED DECISION**. Do not guess or re-prompt around the same gap.
- Before implementation, compare constitution, spec, plan, task, relevant architecture, and ADRs. If they conflict, identify the owning artefact and stop until it is updated through the proper process.

## Required Checks

Use `.agent/workflows/pre-implementation-compliance.md` before application-code changes. Use the post-implementation and merge checks in that workflow before claiming task completion or recommending merge.

Use `.agent/workflows/sdd-lifecycle-check.md` before any lifecycle-stage action. Later-stage artefacts must not be used to justify earlier-stage decisions.
