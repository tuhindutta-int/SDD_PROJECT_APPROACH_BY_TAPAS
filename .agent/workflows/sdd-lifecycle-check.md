# SDD Lifecycle Check

## Purpose

Evaluate a requested action against the current feature lifecycle state before doing the action. The authoritative state model is `.ai-context/lifecycle.md`.

## Required Inputs

- **Feature/spec ID:** `<feature-slug>` or `Not specified`
- **Requested action:** `<action>`
- **Current state:** from `.ai-context/specs/<feature-slug>.spec.md` and `.ai-context/status.md`
- **Upstream artefact:** `<BRD/spec/plan/tasks/test evidence/diff/release record>`
- **Required gate and evidence:** `<Gate 1, architecture check, RED, Gate 2, release evidence>`

## Procedure

1. Identify the current explicit state; never infer it from code, a file's existence, or chat context.
2. Map the requested action to its lifecycle stage using `.ai-context/lifecycle.md`.
3. Verify all required upstream artefacts, human roles, approvals, evidence, and state-transition conditions.
4. Compare constitution, spec, plan, task, architecture, and relevant ADRs when technical execution is proposed.
5. Determine the next valid state, blockers, and required human role.

## Outcome

- **Proceed:** only when the requested action belongs to the current next valid stage and evidence is complete.
- **Stop:** if the action belongs to a later stage, state transition evidence is missing, the state is ambiguous, or artefacts conflict. Report: current state, required input/evidence, next valid state, blockers, and responsible human role.
- **Return upstream:** when a later stage exposes a requirement, intent, or decision problem, identify the artefact that owns it and require that artefact to be updated.

## Stage-Specific Agent Behaviour

| Stage | Agent may do | Agent must not do |
| --- | --- | --- |
| Discovery | Clarify and capture validated BRD context | Implement |
| Spec | Formalize intent, testable ACs, and API contract where applicable | Implement or self-approve |
| Gate 1 | Analyze ambiguity, testability, scope, and conflicts | Declare human approval |
| Plan / Architecture Check | Derive technical approach and identify ADR needs | Add product requirements |
| Tasks | Decompose bounded traceable work | Execute an entire task list unattended |
| Test-first | Create/verify tests and confirm RED | Implement before RED |
| Implementation | Execute one approved task with scoped context and verify GREEN | Expand scope or redefine upstream artefacts |
| Gate 2 | Inspect diff, ACs, security, architecture, and docs | Self-declare human approval |
| Release | Verify readiness and update release/status records | Treat merge as automatic release |
| Production Support | Analyze findings and feed them to the governing artefact | Encode new behaviour only in code |
