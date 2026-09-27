# Architecture

## Governance

This is a living system-design document. It must accurately reflect current
architecture — not aspirational or stale state.

**Update triggers:** When a plan introduces a new integration, new datastore,
significant architectural decision, or meaningful service/component boundary
change, this file must be updated as part of the same SDD change — not
deferred to a later clean-up session.

**How to update:** Run `.agent/workflows/architecture-check.md` to identify
required updates. Record significant architectural decisions in
`.ai-context/decisions/ADR-NNNN-<slug>.md` using the ADR template
(`.ai-context/decisions/_template.adr.md`) and reference them in the
`Architectural Decisions` section below.

**Stale architecture documentation is a governance risk.** An agent session
reading stale context may produce incorrect plans, incorrect ADR assessments,
and incorrect task scope.

Governing rule: `.agent/rules/int-sdd-architecture.md`

## Status

Architecture has not been designed. All areas below are **TBD** pending
validated requirements and approved decisions.


## System Context

TBD

## Application Architecture

TBD

## Frontend

TBD

## Backend

TBD

## Data Layer

TBD

## External Integrations

TBD

## Authentication / Authorization

TBD

## Infrastructure

TBD

## Deployment

TBD

## Observability

TBD

## Performance

TBD

## Security

TBD

## Architectural Decisions

No architectural decisions have been recorded. Future approved decisions belong in `.ai-context/decisions/` and should be referenced here.
