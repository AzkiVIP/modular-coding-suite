---
name: debugging
description: Diagnoses software failures by testing evidence-based hypotheses, locating the point where actual behavior diverges from expected behavior, and coordinating or applying targeted fixes. Use when a bug's cause is unknown, behavior is unexpected, failures are intermittent, or a fix needs root-cause confirmation. For an already-known, localized cause, proceed directly to a minimal fix and verification instead of unnecessary investigation.
---

# Debugging

Diagnose before fixing. A change made before its cause is established is a hypothesis, not a confirmed solution. For non-trivial failures, define the failure, form plausible explanations grounded in project evidence, run the cheapest useful test that distinguishes them, then fix and verify. Do not manufacture investigative ceremony for a known, trivial cause.

## Quick Start

**Is the cause already known with reasonable certainty?** For example, the latest edit introduced a visible typo or an error clearly identifies a misconfigured value that has been confirmed in the relevant file.

- **Yes:** make the smallest appropriate fix, then verify it.
- **No:** follow the diagnostic loop below.

1. **Define** expected versus actual behavior, impact, reproduction steps, and available evidence.
2. **Hypothesize** two to four plausible causes that fit the evidence and actual project structure.
3. **Discriminate** between them using the lowest-cost, lowest-risk check that can change the diagnosis.
4. **Locate** where actual behavior first diverges from expected behavior.
5. **Fix** the confirmed cause with the smallest targeted change.
6. **Verify** the original scenario, relevant regression risks, and any remaining uncertainty.

Never treat an untested guess as a confirmed root cause.

## Workflow

### 1. Define the failure

Establish the expected behavior, actual behavior, impact and scope, reproduction conditions, and evidence available. Capture exact errors or stack traces when useful. Scope may depend on user, input, environment, platform, timing, or load.

Inspect only relevant source files, recent changes, configuration, dependency versions, logs, traces, and tests. Do not infer behavior from filenames or assume that a symptom has the same cause as a previous bug. If the issue cannot be reproduced, use **Intermittent or Unreproduced Failures** below instead of guessing.

### 2. Form plausible hypotheses

List only explanations consistent with the observed evidence and the actual system architecture. Discard explanations that fail to account for important symptoms. For each remaining hypothesis, identify what evidence would support it and what would refute it.

Do not generate a long list of generic possibilities just to appear thorough. A clear, known cause may need no hypothesis list.

### 3. Test before fixing

Choose the single cheapest useful check that can distinguish the strongest competing explanations. Depending on the problem, this may be a targeted test, log inspection, breakpoint, variable or call-stack inspection, API request/response comparison, configuration check, targeted script, Git diff, or history inspection.

Record the actual outcome, not what was expected. Change one relevant variable at a time. Do not apply multiple speculative fixes in parallel: if the symptom disappears, that would not establish which change helped or whether the actual cause was corrected.

A change occurring before a failure is evidence to investigate, not proof of causation. Use tools such as `git log`, `git diff`, `git blame`, or `git bisect` when repository history is relevant and the tool is available. Choose the technique that answers the question; do not use expensive tools when a simple test can settle it.

### 4. Locate the failure boundary

Find where actual behavior first diverges from expected behavior, not merely where the error becomes visible. Trace only the boundaries implicated by evidence, such as input transformation, state updates, API contracts, database operations, asynchronous ordering, configuration, environment differences, or external services.

If the evidence disproves the initial diagnosis or points to another layer, stop changes based on the old hypothesis, update the diagnosis, and route the confirmed issue to the appropriate specialist. Do not keep patching the wrong layer because work has already started.

### 5. Apply or coordinate the fix

Make the smallest change that addresses the confirmed cause while respecting the user's scope and existing project conventions. Avoid unrelated refactors, new dependencies without justification, blanket exception handling, arbitrary delays or retries, and symptom suppression that leaves the underlying problem unresolved.

If the fix belongs to another specialist's domain, provide the confirmed diagnosis, supporting evidence, and affected boundary. Use the host's actual skill or agent mechanism if available. If the specialist instructions cannot be loaded or invoked, do not claim that the specialist was used; apply only the relevant knowledge actually available and disclose the limitation when it materially affects confidence or safety.

**Incident exception:** if a failure is actively causing substantial harm—such as data loss, an active security exposure, or a production outage—prioritize safe containment (for example, rollback, disabling the affected feature, or limiting traffic) before completing the full diagnosis. Preserve useful evidence when feasible, then continue the root-cause investigation. Containment alone is not proof that the cause was fixed.

### 6. Verify and prevent regression

Scale verification to the impact and risk of the fix. Where feasible:

- Confirm the original failure before the fix and repeat the same scenario afterward.
- Add or update a regression test that targets the specific defect.
- Run relevant existing tests and checks for directly affected integrations.
- Inspect the final diff for unintended changes and adjacent regressions.
- For intermittent failures, use repeated trials or targeted instrumentation proportionate to the observed frequency and impact.

If reproduction or automated verification is impossible, state what was inspected, what evidence supports the diagnosis, and what remains unverified. Never claim success solely because code changed or the application starts.

## Intermittent or Unreproduced Failures

When the bug cannot currently be reproduced:

1. Gather available records of actual occurrences: timestamps, inputs, request or trace IDs, environment, load, and relevant logs.
2. Look for a specific trigger or shared condition, such as timing, concurrency, particular input, or environment differences.
3. Form a testable hypothesis about that condition rather than labeling the issue merely “flaky.”
4. If impact justifies it, add narrow instrumentation or a targeted concurrency/reproduction test designed to capture the suspected condition. Do not repeatedly run the entire test suite without a clear hypothesis.
5. State whether the diagnosis is confirmed or remains unconfirmed. Explain what evidence would resolve the uncertainty if the issue recurs.

Do not declare an intermittent bug fixed after only a few clean runs if that number of trials cannot meaningfully challenge the original failure rate.

## Specialist Coordination

Diagnose first; route according to evidence, not the initial guess.

| Specialist | Involve when evidence indicates... |
|---|---|
| `frontend` | Client rendering, interaction, state, or data-fetching logic causes the failure. |
| `backend-data` | API logic, persistence, transactions, or data integrity causes the failure. |
| `architecture` | Structural boundaries or dependencies cause the failure, rather than a local defect. |
| `performance` | A measurable or systemic performance bottleneck is established or requires specialist investigation. |
| `security` | The failure involves or enables an authentication, authorization, confidentiality, or integrity weakness. |
| `testing-quality` | The fix has meaningful integration impact or needs a broader regression strategy. |

Involve only the specialists justified by evidence. Do not invoke multiple specialists speculatively.

## Tool Discipline

Use only tools available in the current environment, such as a debugger, browser DevTools, terminal, Git, logs, or test runner. Never fabricate command output, traces, or test results. Redact credentials, tokens, passwords, and private user data from diagnostic output and reports.

## Examples

### Example 1 — Deterministic bug with an obvious cause

**Input:** A stack trace identifies an access to `user.profile.id`. Inspection shows that `fetchUser()` can return `null` on a 404, and the caller dereferences the result without handling that documented outcome.

**Process:** Confirm the function's return contract and the missing branch. Because the cause is established by the stack trace and source, skip a broad hypothesis workflow.

**Fix:** Handle the not-found result explicitly using the application's existing error or empty-state convention; do not simply hide unrelated exceptions.

**Verify:** Add or update a test for the `null`/not-found path and run relevant tests. Report any checks that were not run.

### Example 2 — Runtime failure with an unknown cause

**Input:** A checkout endpoint intermittently returns HTTP 500. Logs from a failure contain a database deadlock message.

**Process:** Consider plausible causes, including transactions locking `orders` and `inventory` in different orders, a long-running query holding locks, or another database contention issue. Inspect database lock/wait evidence during an occurrence and trace the transactions involved. Suppose the evidence confirms inconsistent lock ordering between `checkout()` and `restock()`.

**Fix:** Make the affected transactions acquire locks in a consistent order, following the database and project conventions. Do not add arbitrary delays or retries as a substitute for addressing the confirmed cause.

**Verify:** Re-run a targeted concurrent test, confirm the original failure no longer occurs in that scenario, check the affected transaction tests, and report the scope and limits of the test evidence.

### Example 3 — Intermittent failure cannot currently be reproduced

**Input:** Users report that a dashboard occasionally shows stale data after an update, but the issue does not occur during a local reproduction attempt.

**Process:** Gather timestamps, request IDs, relevant API responses, cache/state-update logs, and environment details from reported occurrences. Form a hypothesis only if the evidence points to a trigger—for example, a stale response arriving after a newer update. If evidence is insufficient, add narrowly scoped diagnostic logging or instrumentation rather than changing cache logic speculatively.

**Output and verification:** Report the strongest supported diagnosis as unconfirmed until evidence distinguishes it from other causes. If a triggering sequence is captured, reproduce that sequence, make a targeted fix, and test the same sequence before and after. Otherwise, state exactly what evidence should be captured next and do not claim the issue is fixed.

### Example 4 — Specialist needed, but invocation is unavailable

**Input:** Investigation confirms that a frontend symptom is caused by a backend endpoint returning a response shape that violates the documented contract. The current AI environment cannot load or invoke sibling specialist skills.

**Process:** Stop frontend-only speculative changes. Record the confirmed mismatch, evidence, expected contract, actual response, and affected consumer. Do not say that `backend-data` was invoked or that its complete skill instructions were followed.

**Output:** State that specialist invocation is unavailable in this environment. Provide the actionable backend diagnosis and apply only backend guidance supported by the context and accessible project files. Flag any material limitation, implement a targeted correction only if authorized and sufficiently understood, then verify the API response and affected frontend integration where possible.

### Example 5 — Search icon overlaps placeholder text

**Input:** A search field shows its magnifying-glass icon on top of the placeholder or typed text.

**Process:** Reproduce the issue at the affected viewport. Inspect the icon's position and dimensions, the input's computed padding, and all matching responsive rules. Check cascade order and specificity—not just whether an intended `padding-left` declaration exists. For example, `.field--search input` and `input[type="search"]` can have equal specificity, so a later shorthand `padding: 9px 12px` can override the earlier `padding-left` intended to reserve space for the icon.

**Fix:** Change the conflicting rule or use a layout structure that explicitly reserves room for the icon. Keep the approved visual design and avoid adding escalating overrides without understanding the cascade.

**Verify:** Confirm the computed text inset actually changed and inspect both an empty placeholder and typed text at narrow and desktop widths. Add a focused regression check. If browser rendering is unavailable, report the source-level finding and leave visual confirmation unverified.

## Common Pitfalls

- Patching the line where an error appears without tracing where the bad value or state originated.
- Treating a plausible explanation or a recent code change as proven causation.
- Applying several speculative fixes at once, making results uninterpretable.
- Adding blanket `try/catch`, arbitrary delays, or retries that hide the symptom.
- Repeating broad tests without a discriminating hypothesis for an intermittent failure.
- Continuing to modify the wrong layer after evidence changes the diagnosis.
- Claiming a fix, specialist invocation, or test result that was not actually verified.
