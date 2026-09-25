# Generate Plan Workflow

## Purpose

Create a reviewable implementation plan only for an approved feature specification. Do not implement while planning.

## Inputs

- An explicit feature slug.
- `.ai-context/specs/<slug>.spec.md` with approved Gate 1 status.
- Relevant approved decisions and constitution constraints.
- Referenced sections of `.ai-context/architecture.md`.

## Procedure

1. Read the approved spec, applicable constitution constraints, referenced architecture, and only related decisions or modules.
2. Identify explicitly specified integrations, data-model effects, API contracts, dependencies, risks, and deferred scope.
3. Trace each plan element to explicit acceptance criteria or an approved decision.
4. State sequencing and verification points in increments that can be reviewed before task generation.
5. Do not infer requirements, architecture, APIs, data models, technologies, or integrations that are unspecified.
6. Mark missing information as **Not specified**, **Conflicting**, **Requires decision**, or **TBD**; report it rather than guessing.
7. Write the plan to `.ai-context/plans/<slug>.plan.md` only when authorized by the lifecycle state.

## Completion Check

Verify that every proposed action supports identified acceptance criteria and that no out-of-scope work has been added. Do not generate tasks or implementation as part of this workflow; task generation begins only from the resulting approved plan.
