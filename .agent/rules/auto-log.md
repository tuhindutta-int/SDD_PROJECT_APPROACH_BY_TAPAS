# Automatic Work Logging Rule

## Requirement

After completing material agent work, read `.ai-context/prompt_history.md` and append a concise audit entry. Do not rewrite, reorder, or remove earlier entries.

## Entry Content

Record, where applicable:

- Date/time
- Task, spec, or other artefact identifier
- Objective
- Files changed
- Validation performed
- Unresolved issues
- Important decisions or approved deviations

## Safety and Scope

- Do not record secrets, credentials, tokens, PII, or confidential customer data.
- Screen the proposed entry before writing; redact or replace sensitive content with a safe placeholder and record only that exclusion when material.
- Reference authoritative repository artefacts by relative path instead of duplicating their content.
- For a localized task, log only the information necessary to audit the work.
