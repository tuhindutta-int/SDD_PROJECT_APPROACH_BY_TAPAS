# Security Check Workflow

## Authority

This is the security validation workflow for INT SDD Line 11.

Governing rule: `.agent/rules/int-sdd-security.md`
QA governance: `.agent/rules/int-sdd-qa.md`
QA validation: `.agent/workflows/qa-validation-check.md`
Non-negotiables: `.agent/rules/int-sdd-non-negotiables.md` §4
Code review: `.agent/workflows/code-review.md` §5
Architecture governance: `.agent/rules/int-sdd-architecture.md`
Lifecycle (Gate 2): `.ai-context/lifecycle.md`

---

## When to Run

- **Part 1** (Secrets/PII): Before committing any artefact, code, or test.
- **Part 2** (Dependency Vetting): Before any dependency is added to the manifest.
- **Part 3** (Scanning Controls): At Gate 2 or when verifying pipeline outputs.
- **Part 4** (Injection): At Gate 2 for any task with applicable interfaces.
- **Part 5** (Authentication/Authorization): At Gate 2 for any task with auth surface.
- **Part 6** (Least Privilege): At Gate 2 for any task that introduces permissions.
- **Part 7** (Data Protection): At Gate 2 for any task handling sensitive data.
- **Part 8** (Gate 2 Security Evidence): As part of full Gate 2 preparation.
- **Full check**: When performing a security audit.

---

## OUTPUT FORMAT

Produce one of:

```
SECURITY CHECK PASSED
```

```
SECURITY CHECK — BLOCKING: <findings>
SECURITY CHECK — WARNING: <findings>
SECURITY CHECK — NOT APPLICABLE: <control> — <justification>
```

Use **BLOCKING** for issues that prevent Gate 2 approval or commit.
Use **WARNING** for issues requiring human attention.
Use **NOT APPLICABLE** only with explicit written justification per control.

`Not Applicable` without justification is NOT a valid outcome.

---

## Part 1 — Secrets and PII Check

_Run before committing any artefact, code, test fixture, configuration, or prompt._

### 1.1 Artefact content scan

For every file being committed or added to a repository artefact:

- [ ] **[BLOCKING]** No API keys, access tokens, or credentials present.
- [ ] **[BLOCKING]** No passwords or private keys present.
- [ ] **[BLOCKING]** No session secrets present.
- [ ] **[BLOCKING]** No customer PII (personal names, emails, phone numbers,
      government IDs, payment data) present.
- [ ] **[BLOCKING]** No production secrets in any form present.
- [ ] **[BLOCKING]** No `.env` files with real credentials committed.

### 1.2 Prompt and prompt history

- [ ] **[BLOCKING]** Prompt content does not contain real credentials.
- [ ] **[BLOCKING]** Prompt history entries (`prompt_history.md`) do not contain
      real secrets or PII from the project.

### 1.3 Test fixtures and synthetic data

- [ ] **[BLOCKING]** Test fixtures use synthetic data, not real customer/production data.
- [ ] **[BLOCKING]** Synthetic data is clearly non-real
      (e.g., `test@example.com`, `0000-0000-0000-0000`, `<REDACTED>`).
- [ ] **[WARNING]** Any sensitive-like structure in test data is documented as
      synthetic.

### 1.4 Log output and commit messages

- [ ] **[BLOCKING]** Generated log output does not contain secrets or PII.
- [ ] **[BLOCKING]** Commit messages do not contain secrets or PII.

**Part 1 result:**

```
SECRETS/PII CHECK: Clean / BLOCKING [list findings]
```

---

## Part 2 — Dependency Vetting

_Run before adding any dependency to the project manifest._

For each proposed dependency:

### 2.1 Legitimacy

- [ ] **[BLOCKING]** Package exists on the expected official registry.
- [ ] **[BLOCKING]** Package name is correct; not a typosquat of a well-known package.
- [ ] **[BLOCKING]** Package is published by the expected maintainer/organization.

### 2.2 Purpose and necessity

- [ ] **[BLOCKING]** Package does what is claimed and is appropriate for the task.
- [ ] **[BLOCKING]** Package is actually necessary; existing dependencies cannot
      satisfy the requirement.

### 2.3 Security posture

- [ ] **[BLOCKING]** Package has no current high or critical CVEs without an
      accepted mitigation.
- [ ] **[WARNING]** Package has no medium CVEs or documented medium risks.
- [ ] **[WARNING]** Package is actively maintained (recent commits; issues addressed).
- [ ] **[WARNING]** Package is not archived or deprecated.

### 2.4 Transitive dependencies

- [ ] **[WARNING]** Transitive dependency chain does not introduce known prohibited
      or vulnerable packages.

### 2.5 Licensing

- [ ] **[WARNING if no policy defined]** License is compatible with the project
      licensing policy when established.

### 2.6 Human acceptance

- [ ] **[BLOCKING]** A human engineer has reviewed and accepted this dependency.
- [ ] **[BLOCKING]** "AI suggested it" alone does not constitute acceptance.

Record the vetting outcome:

```
Dependency:          <package-name@version>
Registry:            <npm / PyPI / Maven / other>
Legitimacy:          Verified / FAILED
Purpose verified:    Yes / No
CVEs:                None / [list with severity]
Maintained:          Yes / No
Human acceptance:    <name> / PENDING
Vetting date:        YYYY-MM-DD
```

**Part 2 result:**

```
DEPENDENCY VETTING: <package-name> — Accepted / BLOCKED / Pending human review
```

---

## Part 3 — Scanning Controls

_Run at Gate 2 or when pipeline scan results are available._

### 3.1 SAST (Static Application Security Testing)

- [ ] **[BLOCKING]** SAST has been run against the changeset.
  - If pipeline exists: attach result summary with version and timestamp.
  - If pipeline not yet established: document as `DEFERRED — pipeline pending`.
    Do not mark as `Passed` when no scanner has run.
- [ ] **[BLOCKING if run]** SAST result shows no high or critical findings without
      an accepted, documented security exception.
- [ ] **[WARNING if run]** SAST result shows no medium findings without review.

### 3.2 DAST (Dynamic Application Security Testing)

Determine applicability:

- `Applicable` — feature has an externally or internally reachable interface
  that can be probed dynamically.
- `Not Applicable with justification` — feature has no reachable interface,
  is a backend-only library, or cannot be probed dynamically.

If applicable:

- [ ] **[BLOCKING]** DAST has been run against the deployed test environment.
  - If pipeline exists: attach result summary.
  - If pipeline not yet established: document as `DEFERRED — pipeline pending`.
- [ ] **[BLOCKING if run]** DAST result shows no high or critical findings without
      an accepted, documented security exception.

### 3.3 Dependency scanning

- [ ] **[BLOCKING]** Dependency scan has been run.
  - If pipeline exists: attach result summary.
  - If pipeline not yet established: document dependency vetting from Part 2 as
    the current control; mark automated scan as `DEFERRED`.
- [ ] **[BLOCKING if run]** Scan result shows no high or critical CVEs without
      an accepted, documented security exception.

**Part 3 result:**

```
SAST:                 Passed / Failed / Deferred (pipeline pending)
DAST:                 Passed / Failed / Not Applicable (<reason>) / Deferred
Dependency scan:      Passed / Failed / Deferred (manual vetting in Part 2)
```

---

## Part 4 — Injection Check

_Run at Gate 2 for tasks with applicable interfaces._

### 4.1 Determine applicability per injection class

For each class, state `Applicable` or `Not Applicable — <reason>`.

| Injection class | Applicable? | Result |
| --------------- | ----------- | ------ |
| SQL / NoSQL injection | | |
| Command injection | | |
| Template injection | | |
| Cross-site scripting (XSS) | | |
| Path traversal | | |
| LDAP injection | | |
| Prompt injection (AI components) | | |
| Other context-specific classes | | |

### 4.2 Injection verification

For each applicable class:

- [ ] **[BLOCKING]** Inputs from users or external sources are validated and
      sanitized before reaching the sensitive operation.
- [ ] **[BLOCKING]** Parameterized queries, prepared statements, or equivalent
      safe patterns are used where injection could occur.
- [ ] **[BLOCKING]** Output is correctly encoded before rendering in the target context.
- [ ] **[BLOCKING (AI systems)]** External content (user input, API responses,
      retrieved data) is not silently promoted to trusted instruction status.

**Part 4 result:**

```
INJECTION CHECK: <feature-slug>
Applicable classes:    [list]
Not applicable:        [list with justification]
Result:                Passed / BLOCKED [findings]
```

---

## Part 5 — Authentication and Authorization Check

_Run at Gate 2 for tasks with authentication or authorization surface._

### 5.1 Applicability

State `Not applicable — <reason>` if the feature has no authentication or
authorization surface. Do not skip without explicit justification.

### 5.2 Authentication check

- [ ] **[BLOCKING]** Authentication is implemented at the correct system boundary.
- [ ] **[BLOCKING]** Authentication mechanism is consistent with the approved architecture.
- [ ] **[BLOCKING]** Authentication cannot be bypassed by a caller.
- [ ] **[BLOCKING]** Token/session handling is secure (no exposure in URLs, logs, responses).

### 5.3 Authorization check

- [ ] **[BLOCKING]** Authorization enforcement exists at every privilege-requiring action.
- [ ] **[BLOCKING]** Authorization is not assumed because authentication occurred.
- [ ] **[BLOCKING]** User-controlled identifiers cannot cross ownership boundaries
      (IDOR risk assessed).
- [ ] **[BLOCKING]** Service-to-service permissions are enforced and appropriately scoped.
- [ ] **[BLOCKING]** Authorization failures return appropriate responses and do not
      leak data about the resource.
- [ ] **[BLOCKING]** Unauthenticated and unprivileged paths are correctly restricted.

**Part 5 result:**

```
AUTH CHECK: <feature-slug>
Authentication:        Verified / BLOCKED [findings] / Not applicable
Authorization:         Verified / BLOCKED [findings] / Not applicable
```

---

## Part 6 — Least Privilege Check

_Run at Gate 2 for tasks that introduce or modify permissions._

### 6.1 Applicability

State `Not applicable — <reason>` if the task introduces no permissions.

### 6.2 Permission review

For each identity or permission set introduced or modified by this task:

| Subject | Required permissions | Actual permissions granted | Compliant? |
| ------- | -------------------- | -------------------------- | ---------- |
| `<identity>` | `<what it needs>` | `<what it has>` | `Yes / No` |

- [ ] **[BLOCKING]** No identity has been granted more permissions than required
      by the approved task and system behaviour.
- [ ] **[BLOCKING]** Application identity has only the permissions the application
      actually requires.
- [ ] **[BLOCKING]** Database user has only required read/write/delete access
      for its operations.
- [ ] **[BLOCKING]** API credentials are narrowly scoped to required operations.
- [ ] **[BLOCKING]** External integration credentials are scoped to required operations.
- [ ] **[WARNING]** Service accounts are reviewed for accumulation of unnecessary
      historic permissions.

**Part 6 result:**

```
LEAST PRIVILEGE CHECK: <feature-slug>
Identities reviewed:   [list]
Result:                Compliant / BLOCKED [findings] / Not applicable
```

---

## Part 7 — Data Protection Check

_Run at Gate 2 for tasks that handle, store, or transmit sensitive data._

### 7.1 Applicability

State `Not applicable — <reason>` where a data-protection check does not apply.

### 7.2 Data at rest

- [ ] **[BLOCKING]** Sensitive data stored by this feature is encrypted at rest
      where required by the approved architecture.
- [ ] **[BLOCKING]** No unapproved data store introduced.
- [ ] **[BLOCKING]** Data not retained beyond its required lifecycle.

### 7.3 Data in transit

- [ ] **[BLOCKING]** Sensitive data transmitted by this feature uses approved
      encryption in transit (e.g., TLS where applicable).
- [ ] **[BLOCKING]** Certificate validation is not bypassed.
- [ ] **[BLOCKING]** Sensitive data not transmitted over unapproved channels.

### 7.4 Logs and secondary exposure

- [ ] **[BLOCKING]** Log output does not contain passwords, credentials, session
      tokens, PII, or API keys.
- [ ] **[WARNING]** Cache contents are appropriately scoped and protected.
- [ ] **[WARNING]** Temporary files containing sensitive data are handled and cleaned up.
- [ ] **[WARNING]** Exports are appropriately scoped and protected.
- [ ] **[WARNING]** Third-party integrations do not receive unnecessary sensitive data.

**Part 7 result:**

```
DATA PROTECTION CHECK: <feature-slug>
Data at rest:          Verified / BLOCKED [findings] / Not applicable
Data in transit:       Verified / BLOCKED [findings] / Not applicable
Log exposure:          Verified / BLOCKED [findings] / Not applicable
```

---

## Part 8 — Gate 2 Security Evidence

_Run as part of full Gate 2 preparation. Aggregates Parts 1–7._

### 8.1 Security checklist completion

For each of the 16 Gate 2 security controls (see `int-sdd-security.md` §10.1):

| Control | Applicable | Result | Evidence reference |
| ------- | ---------- | ------ | ------------------ |
| 1. Injection | | | |
| 2. Authentication | | | |
| 3. Authorization | | | |
| 4. Secret handling | | | |
| 5. Least privilege | | | |
| 6. Data at rest | | | |
| 7. Data in transit | | | |
| 8. Dependency vetting | | | |
| 9. SAST | | | |
| 10. DAST | | | |
| 11. Dependency scan | | | |
| 12. PII handling | | | |
| 13. Logging/PII exposure | | | |
| 14. Security configuration | | | |
| 15. Architecture/ADR threats | | | |
| 16. Prompt injection / untrusted input (if AI) | | | |

- [ ] **[BLOCKING]** All applicable controls have result `Passed` or a documented
      security exception.
- [ ] **[BLOCKING]** All `Not Applicable` entries have written justification.
- [ ] **[BLOCKING]** Any security exception has been formally documented and
      signed off per governance (see `int-sdd-security.md` §9).
- [ ] **[BLOCKING]** No security control is marked `Passed` when no scanner has run
      and tooling is deferred.

### 8.2 Security evidence record format

Include in the Gate 2 evidence record:

```
Feature slug:              <slug>
Security checklist:        Complete / Incomplete
SAST result:               Passed / Failed / Deferred (pipeline pending)
DAST result:               Passed / Failed / Not applicable / Deferred
Dependency scan result:    Passed / Failed / Deferred (manual vetting performed)
Injection classes checked: [list applicable, list N/A with reason]
Auth/authz verified:       Yes / Not applicable
Least privilege verified:  Yes / Not applicable
Data protection verified:  Yes / Not applicable
Security exceptions:       None / [reference to exception records]
Security reviewer/owner:   <name or role>
Date:                      YYYY-MM-DD
```

**Part 8 result:**

```
GATE 2 SECURITY EVIDENCE: <feature-slug>
Status: Ready / Not ready
Blocking items: [list or None]
```

---

## Security Exception Record Format

When a security control cannot be satisfied:

```
SECURITY EXCEPTION:
Feature slug:    <slug>
Control:         <which control>
Finding:         <what the specific issue is>
Risk:            <what security risk this creates>
Owner:           <human who owns this exception>
Remediation:     <what must be done to close the exception>
Deadline:        <when remediation must be completed>
Sign-off:        <human who authorized this exception>
Sign-off date:   YYYY-MM-DD
Reference:       <exception record ID or location>
```

An agent cannot complete the Sign-off field. Human sign-off is required.

---

## Findings Format

For each finding:

```
ID:                  SEC-<AREA>-NNN
Severity:            BLOCKING | WARNING
Area:                <area code>
Observation:         <what was found>
Why it matters:      <what security control is violated>
Evidence:            <specific location, ID, or absent item>
Required action:     <what must be done>
```

Area codes:

| Code | Area |
| ---- | ---- |
| PII | Secrets and PII |
| DEP | Dependency |
| SCN | Scanning (SAST/DAST/dependency) |
| INJ | Injection |
| AUT | Authentication |
| AUZ | Authorization |
| PRI | Least privilege |
| DAR | Data at rest |
| DIT | Data in transit |
| LOG | Logging/PII exposure |
| EXC | Security exception |
| UNT | Untrusted/external content |

---

## Scenario Quick Reference

| Scenario | Expected result |
| -------- | --------------- |
| E: Security scanning skipped because AI generated the code | BLOCKING — Part 3; security is authorship-independent |
| F: AI-suggested dependency auto-installed | BLOCKING — Part 2; dependency vetting required |
| G: SAST finds security issue | BLOCKING — Part 3; Gate 2 security failure |
| H: DAST not applicable (no external interface) | NOT APPLICABLE with documented justification |
| I: API auth exists but authorization boundary not verified | BLOCKING — Part 5 |
| J: Application identity has excess permissions | BLOCKING — Part 6; least-privilege failure |
| K: Secrets appear in generated logs | BLOCKING — Part 1 / Part 7 |
| L: Sensitive data transmitted insecurely | BLOCKING — Part 7 |
| M: Sensitive data stored without approved protection | BLOCKING — Part 7 |
| N: External content injected into AI prompt as trusted instruction | BLOCKING — Part 4; prompt injection risk |
| P: Security exception verbally agreed but undocumented | NON-COMPLIANT — Part 8; formal record and sign-off required |
| Q: Dependency scan unavailable; no CI pipeline yet | DEFERRED — document; controls not waived |
