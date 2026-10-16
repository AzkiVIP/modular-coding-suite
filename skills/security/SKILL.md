---
name: security
description: Assesses and mitigates application security risks using evidence from actual code, configuration, dependencies, and trust boundaries. Use for authentication or authorization changes, sensitive data access, untrusted input, security findings, exposed secrets, dependency vulnerabilities, security-sensitive configuration, or when a change materially affects the attack surface. Do not run a full security review for routine changes with no meaningful security impact.
---

# Security

Make security decisions from evidence, not generic checklists. Scale investigation depth to data sensitivity, exposure, impact, and reversibility. A one-line CSS change does not need a security review; an authorization change requires investigation of the affected trust boundary.

## Quick Start

Before touching code, determine:

1. What is changing: feature, endpoint, dependency, data flow, or configuration?
2. Does it cross a meaningful trust boundary or affect authentication, authorization, sensitive data, untrusted input, or externally reachable behavior?
3. Which existing controls protect that boundary? Inspect rather than assuming they exist or are absent.
4. What is the smallest scope of investigation and verification that addresses the actual risk?

If there is no meaningful security impact, proceed with normal engineering care. Otherwise, use the workflow below, scoped to the boundary being changed.

## Workflow

- [ ] **Scope:** Identify relevant assets, trust boundaries, entry points, privileges, and sensitive data.
- [ ] **Threats:** Identify plausible attack paths an actor could actually take.
- [ ] **Investigate:** Inspect the real code/configuration, establish root cause, and assess exploitability.
- [ ] **Remediate:** Apply the smallest effective control that addresses the cause.
- [ ] **Verify:** Confirm the risk is reduced without breaking legitimate behavior.
- [ ] **Report:** State findings, confidence, residual risk, and what remains unverified.

#### Persisted and client-side data validation

Treat local browser storage, imported JSON, URL state, and values restored from caches as potentially malformed or modified input. Persistence does not make data trustworthy.

- Validate the expected schema, primitive types, required fields, allowed enum values, and reasonable size/range constraints before using persisted records.
- For fixed maps of allowed values, use an own-property check such as `Object.hasOwn(map, value)` (or `Object.prototype.hasOwnProperty.call(map, value)` where compatibility requires it). Do not use `value in map` as an enum allow-list check because inherited names can pass.
- Normalize or reject unknown values deterministically. Avoid merging untrusted objects into configuration/prototype-bearing objects without safe handling.
- Keep client-side validation proportional to the app. Browser storage is not an authorization boundary; if the data is sent to a server or reaches a sensitive operation, validate again at the authoritative boundary.
- Test malformed records, unexpected enum strings (including inherited property names), missing fields, and legitimate records. Do not describe client-side validation as protection against a user who controls their own browser.

### 1. Scope the context

Read only the code and configuration relevant to the change: authentication and authorization logic, data flow, dependency manifests, exposed routes, and existing controls. Identify what data or operations are sensitive, who can reach the code path and with what privileges, and which mitigations already exist.

### 2. Identify threats proportionately

For a trivial change, a brief check is sufficient. For meaningful changes, consider plausible attackers at different trust levels (anonymous, authenticated user, another tenant's user, administrator). Use OWASP Top 10 and ASVS as references to avoid blind spots, not mandatory checklists for every task.

Prioritize threats by evidence, exploitability, impact, exposure, and remediation needs. Distinguish confirmed vulnerabilities from suspicious patterns, hypothetical risks, and missing evidence.

### 3. Investigate before fixing

- Trace the actual code path; do not fix a symptom that has not been located in the source.
- Assess exploitability under the real configuration and access model. A suspicious pattern may be unreachable or mitigated elsewhere; verify instead of assuming.
- Look for the root cause in shared logic or trust boundaries, not just the single call site where the issue became visible.
- If you cannot reproduce or confirm the risk, state what was inspected and what evidence is still needed. Do not claim confirmed exploitability without evidence, and do not dismiss a credible risk solely because reproduction failed.

### 4. Apply the minimal effective control

Prefer the framework's established secure mechanisms over custom security code. Match the control to the actual threat and avoid unrelated hardening while fixing a specific issue.

| Area | Examples of risk | Relevant controls (select as needed) |
|---|---|---|
| Access control | IDOR, privilege escalation | Enforce server-side authorization for each resource and operation; least privilege; deny by default where appropriate |
| Input and output | SQL injection, XSS, command injection, unsafe deserialization | Parameterized queries, context-aware output encoding, allow-lists, safe APIs; avoid executing untrusted data |
| Secrets and crypto | Leaked credentials, weak cryptography | Rotate exposed live secrets, use appropriate secret storage, established password-hashing and cryptographic libraries; never invent cryptography |
| Web requests and uploads | CSRF, SSRF, open redirects, unsafe uploads | Framework protections, destination/scheme allow-lists where applicable, upload size/type/storage controls |
| Errors and data exposure | Stack traces or sensitive data leaked | Safe client errors, useful sanitized server-side diagnostics, minimal sensitive-data logging |
| Dependencies and delivery | Applicable dependency vulnerabilities, excessive build permissions | Verify affected versions and vulnerable code paths, patch compatibly, least-privilege build/deploy credentials, protect artifact integrity |

### 5. Verify risk reduction

Define success for the finding before declaring it fixed:

- **Authorization:** unauthorized actors are denied and legitimate users can still access their own resources.
- **Injection:** a regression test exercises the relevant code path with hostile input and confirms safe behavior; legitimate input still works.
- **Secret exposure:** determine exposure through history/logs/configuration as relevant, rotate a live secret, and verify the old credential is rejected.
- **Dependency vulnerability:** verify the affected version and whether the vulnerable functionality is reachable, patch compatibly, run relevant checks, and rescan when available.
- **Configuration:** confirm disallowed origins, permissions, or operations are rejected while required legitimate usage remains functional.

A clean scanner or passing test is evidence, not proof of total safety. Record remaining uncertainty and untested paths.

### 6. Coordinate without absorbing other workflows

Hand off when a task requires another domain's full procedure:

- `architecture`: trust-boundary redesign or new service boundaries.
- `backend-data`: implementation of server-side validation, query, or data-access changes.
- `frontend`: safe rendering or client-side session/interface changes.
- `debugging`: the root cause of an unexplained failure is not yet known.
- `testing-quality`: broader regression or integration coverage beyond security-specific checks.
- `performance`: a security control has a material, evidenced performance cost.
- `technical-research`: the applicability, affected version, or official details of an advisory cannot be established from available project evidence.

Only claim a specialist skill was invoked if the platform actually supports and performed that invocation. Otherwise, use the available guidance honestly and disclose material limitations.

## Examples

### Example 1 — Broken access control

**Input:** "User A can view `/api/orders/{id}` for order IDs belonging to User B by changing the ID in the URL."

**Process:** Inspect the handler, session identity, data-access query, and ownership model. Confirm whether the request fetches by ID without verifying the caller's permission for that specific order.

**Fix:** Enforce ownership or role authorization at the appropriate server-side boundary. Return the project's appropriate denial response; use `404` instead of `403` only when consistent with the API convention and needed to avoid revealing resource existence.

**Verify:** User A is denied access to User B's order, while the legitimate owner can still access their own order.

### Example 2 — Suspected SQL injection

**Input:** "The search endpoint concatenates a query parameter into SQL."

**Process:** Inspect the actual query construction and trace whether user-controlled input reaches the query. For example, `f"SELECT * FROM items WHERE name LIKE '%{q}%'"` is unsafe if `q` is inserted directly into SQL.

**Fix:** Use the database driver's parameter binding or the ORM's safe query API (for example, a bound `LIKE` parameter). For dynamic identifiers or sort direction that cannot be bound as ordinary values, use an explicit allow-list.

**Verify:** A regression test confirms hostile input cannot change query meaning or reveal data, while legitimate searches still work.

### Example 3 — Secret found in repository

**Input:** "A configuration file committed to Git appears to contain an API key."

**Process:** Establish whether it is a real live secret rather than a placeholder. Inspect the relevant history and exposure window without copying the secret into reports or logs.

**Fix:** Revoke or rotate the live credential, move secret handling to an appropriate environment/secret manager, prevent future commits, and consider history cleanup where exposure and repository distribution justify it. Removing the current file alone is insufficient.

**Verify:** Confirm the old credential is rejected by the issuing service and document any exposure history that could not be established.

### Example 4 — Dependency vulnerability

**Input:** "A scanner flags an older `lodash` version for a prototype-pollution CVE."

**Process:** Verify the installed/resolved version, advisory applicability, and whether the affected function is reachable with attacker-controlled input.

**Decision:** If applicable code is reachable, prioritize a compatible patched version and test affected behavior. If the vulnerable path is demonstrably unreachable, document the evidence and residual risk; do not call the whole dependency universally safe or ignore the finding without rationale.

**Verify:** Run relevant tests and rescan when tooling is available; report the exact checks performed.

### Example 5 — Report cannot be reproduced

**Input:** "A penetration-test report claims session fixation, but its steps do not reproduce in staging."

**Process:** Inspect session creation and rotation on login and privilege changes, including alternate paths such as SSO callbacks. Check whether staging matches the affected build/configuration.

**Decision:** If the relevant paths rotate session identifiers correctly, report the checked paths and evidence; do not label the report definitively false without verifying its original assumptions. If one path lacks rotation, scope the finding to that path.

**Verify:** State the confidence level, paths checked, and what additional evidence is needed.

### Example 6 — Tightening CORS without breaking legitimate clients

**Input:** "Replace an overly permissive wildcard CORS policy."

**Process:** Identify legitimate frontend origins and other documented consumers, and inspect the existing CORS behavior.

**Fix:** Replace the wildcard with an explicit allow-list of verified legitimate origins when appropriate. Do not confuse CORS with authentication or authorization.

**Verify:** Known legitimate origins continue to work and unauthorized origins are not granted the configured cross-origin access. Escalate unclear consumers rather than guessing.

### Example 7 — Malformed persisted enum

**Input:** "A task loaded from localStorage can contain an unknown status or priority."

**Process:** Trace parsing and normalization, identify the exact accepted enum set, and inspect how values are used in rendering and state transitions. Use an own-property allow-list check instead of `value in map`; validate record shape and types without treating browser storage as authoritative.

**Fix:** Reject or normalize invalid values according to a documented safe default. Do not let malformed data create invalid markup or break unrelated tasks.

**Verify:** Test valid values and unexpected strings such as `toString`, `constructor`, `__proto__`, and a random unknown token. Confirm legitimate records still load and malformed ones are rejected/normalized safely. Activate this skill when untrusted-data risk is material; do not claim a localStorage enum issue automatically constitutes a server-side vulnerability.

### Example 8 — Trivial change with no security impact

**Input:** "Change the margin of a static heading from 8px to 12px."

**Process:** Inspect the relevant component or style token, make the scoped change, and verify it as appropriate. The change does not cross a meaningful trust boundary or affect data handling, permissions, dependencies, or exposed behavior.

**Decision:** No dedicated security review is needed. Do not run the full OWASP checklist for an unrelated visual adjustment.

## Best Practices

- Investigate actual code and configuration before asserting that a vulnerability exists or is fixed.
- Scale depth to impact: do not full-audit a typo fix, and do not skim an authorization change.
- Fix the root cause rather than only the visible call site.
- Prefer established secure framework and library mechanisms over custom implementations.
- Verify both that unauthorized behavior is blocked and that legitimate use still works.
- State confidence, residual risk, and unverified areas explicitly.

## Common Pitfalls

- Running a full security checklist against every change regardless of relevance.
- Declaring a vulnerability or fix without inspecting the real code path.
- Patching one endpoint while leaving the same root cause in shared logic.
- Failing to rotate a secret exposed in Git history, or upgrading dependencies blindly without checking applicability and compatibility.
- Treating a passing scanner as proof that the application is secure.
- Using the JavaScript `in` operator as a fixed-enum allow-list check for untrusted values.
- Trusting localStorage or imported JSON merely because it was previously written by the application.
- Breaking legitimate access while closing a vulnerability.
- Performing intrusive or destructive security testing against production or third-party systems without authorization.
