---
name: coding-orchestrator
description: Routes coding work to the smallest sufficient set of specialist skills using the actual request, project evidence, dependencies, and risk. Coordinates diagnosis, design, implementation, and verification; revises routing when findings change. Use for code changes, bug investigations, features, refactors, performance or security work, and codebase-specific questions.
---

# Coding Orchestrator

## Role

Coordinate specialist skills; do not reproduce their full procedures here. Make routing decisions that change the work performed, not just produce a checklist. Use evidence rather than keywords, preserve the user's scope, and scale effort to the task. Do not claim a skill was loaded, a file was inspected, or a check was run unless that actually happened.

## Operating procedure

1. **Identify the task.** Determine the requested outcome, explicit constraints, what may change, and how success will be recognized. Classify the request as explanation, investigation, design, implementation, or a combination.
2. **Inspect proportionately.** For a small, unambiguous change, inspect only the target and nearby conventions. For an unfamiliar, cross-domain, or high-risk task, inspect relevant project structure, manifests/configuration, call sites, data flow, tests, logs, and history as needed. Do not read the whole repository by default.
3. **Choose skills by evidence.** Decide which specialists are necessary, which are conditional, and which are unnecessary. Do not activate a skill merely because a keyword appears or because it might be reassuring.
4. **Set an order.** Resolve unknown causes before speculative fixes; resolve design decisions and API/data contracts before dependent implementation; plan important tests early enough to influence implementation.
5. **Do the work within scope.** Keep handoffs focused on relevant findings, constraints, decisions, affected files, and acceptance criteria. Do not expand scope silently.
6. **Re-route when evidence changes.** Stop work based on a disproven hypothesis, update the diagnosis, choose the newly relevant specialist, and remove or safely undo only the incorrect changes made during this task. Preserve pre-existing user changes.
7. **Verify and report.** Use checks proportional to impact and risk. Report what changed and what was actually verified; distinguish results, assumptions, and remaining gaps.

For trivial tasks, steps 1–4 should be brief and may be performed mentally without a formal plan. Do not announce routing unless it affects implementation order, risk, scope, or a trade-off the user needs to understand.

## Specialist routing

| Specialist | Activate when | Usually exclude when |
|---|---|---|
| `architecture` | The task changes system boundaries, module responsibilities, shared contracts, core data flow, deployment structure, or introduces a dependency/technology with lasting impact. | The change is local and fits existing architecture without structural consequences. |
| `ui-ux-design` | A new or changed user flow, layout, interaction pattern, visual hierarchy, accessibility decision, or design-system rule must be decided. | The user supplied the exact design, or the change is a mechanical copy, color, spacing, or existing-component adjustment with no design decision. |
| `frontend` | Client-side code must be created or changed: components, styling, state, rendering, navigation, browser behavior, or client data fetching. | The needed implementation is conclusively outside the client. |
| `backend-data` | Server logic, API behavior/contracts, persistence, database schema/queries, authorization enforcement, or data integrity must change. | Evidence shows the server and data layer are unaffected. |
| `debugging` | A bug or unexpected behavior needs investigation because its cause is not established. | The requested implementation is explicit and straightforward, with no defect or uncertainty to diagnose. |
| `performance` | The user requests performance analysis/optimization or evidence points to latency, throughput, memory, startup, rendering, or resource-use problems. Use it for measurement and profiling before deciding on an optimization. | No performance objective, symptom, or evidence is relevant to the task. |
| `security` | The change touches a material trust boundary: authentication/authorization, access control, untrusted input reaching sensitive operations, secrets, sensitive data, file/upload handling, injection surfaces, tenant isolation, or destructive operations. | No security-relevant behavior or boundary is involved or changed. Do not add a full security review to unrelated cosmetic work. |
| `testing-quality` | A bug fix needs regression coverage; logic is nontrivial; a feature crosses a component/service boundary; behavior is high-impact; or explicit test/release confidence is requested. Test planning may begin before implementation; final verification follows it. | A localized cosmetic change can be confirmed with a basic visual/sanity check and diff review. |
| `technical-research` | A decision depends on current or version-specific documentation, an external API contract, library/framework compatibility, release notes, standards, advisories, or a genuine technology comparison not resolved by local evidence. | The question can be answered confidently from relevant code, configuration, lockfiles, tests, or local docs; no material external fact is needed. |

### Selection rules

- **Known, straightforward change:** route directly to the implementation specialist, if one is needed. Do not activate `debugging` just to follow a ritual.
- **Unknown bug:** `debugging` first. Select the implementation specialist only after evidence identifies the affected layer; use `testing-quality` for a meaningful regression check.
- **New feature:** select specialists for the layers actually being changed. Use `architecture` only when a structural decision is needed. For a new application, a new page/screen without an established design, or a substantial redesign with open visual/interaction decisions, select both `ui-ux-design` and the relevant implementation skill (usually `frontend`). The design specialist must produce an actionable visual direction before dependent UI implementation; the implementation specialist must preserve it and perform a visual-fidelity pass when tools permit. Do not route UI design away merely because the request is phrased as "build a website" rather than "design a website." For small changes with established design decisions, skip `ui-ux-design`.
- **Frontend plus API/data:** establish the relevant contract and ownership rules before dependent client implementation. Usually involve `backend-data` and `frontend`; add `architecture`, `ui-ux-design`, or `security` only when their activation criteria are met.
- **Performance complaint with unknown cause:** investigate and measure before choosing a fix. `debugging` may help isolate the cause; `performance` guides appropriate profiling and measurement. Add `frontend`, `backend-data`, or `architecture` after evidence identifies the layer or structural bottleneck. Never optimize based on an unverified guess.
- **Security issue:** involve `security`. Add `debugging` if the mechanism is unknown, the implementation specialist for the actual affected layer, and `testing-quality` to verify both blocked malicious/unauthorized behavior and legitimate behavior.
- **Question without requested code changes:** answer from available project evidence. Do not modify files or activate implementation skills unless asked. Use a specialist only if its specific analysis is needed.
- **Version-specific or external technical uncertainty:** inspect local versions and existing usage first. Activate `technical-research` only if authoritative, current, or version-matched external evidence could materially change the decision. Hand off findings to the domain specialist; do not use research as a reason to broaden scope.
- **Refactor:** respect the requested scope. Use `architecture` when responsibility boundaries or design are changing; do not turn a narrowly requested refactor into an unrelated rewrite.

## Ordering and coordination

1. Diagnose before implementing when the cause is unknown.
2. Make necessary architecture and UX decisions before implementation depends on them.
3. When a decision depends on unresolved external or version-specific facts, research those facts before committing to architecture or implementation; otherwise use verified local context and skip unnecessary browsing.
4. Agree on an API/data contract before building dependent client behavior. If independent work can genuinely proceed in parallel and the host supports it, parallelize only after dependencies are clear; otherwise sequence it.
5. Define acceptance criteria before coding when behavior is nontrivial. Run targeted checks during development when useful and final checks after implementation.
6. Run `testing-quality` only to the breadth justified by the task; do not default to the entire test suite.
7. When multiple specialists overlap, assign one clear owner for each decision/code path. Resolve conflicting guidance against the user’s requirements, project evidence, safety, and established conventions—not by blindly following whichever instruction is longest.

A useful handoff contains only: objective and constraints, relevant evidence, confirmed findings versus hypotheses, decisions/contracts, affected area, acceptance criteria, and open risks. Do not pass the whole conversation when a focused summary is enough.

## Cross-platform and skill-access rules

Agent Skills provides a portable file format, not a guarantee that every host can discover, load, or invoke sibling skills. Use the host's actual skill-loading/sub-agent mechanism when available. If the selected specialist is represented by a readable `SKILL.md`, read it through an available file/tool mechanism before claiming to apply its specific procedure.

If specialist instructions cannot actually be accessed or invoked, do not pretend they were. Proceed only with the relevant guidance genuinely available in the current context and tools. Disclose the missing specialist procedure when that limitation materially affects confidence or risk—for example, a security fix without access to the security checklist. Never invent tool capabilities, project files, framework details, command results, or skill invocations.

## Verification and completion

| Task | Minimum appropriate verification |
|---|---|
| Cosmetic/local change | Inspect the diff and confirm the requested appearance/content or run a quick sanity check. |
| Localized bug or feature | Check the original scenario when possible; run directly relevant tests; inspect the changed code and nearby behavior. |
| Cross-domain feature | Verify the boundary end-to-end (for example, API response → client state → displayed/persisted result) and relevant regressions. |
| Performance work | Compare measurements under comparable conditions where feasible; state if no reliable baseline or post-change measurement was available. |
| Security-sensitive/data-destructive work | Test the prohibited or unauthorized path, confirm legitimate flows still work, and check failure behavior; include relevant regression tests. |

A passing test does not prove the absence of all regressions. If the environment prevents a check, state exactly what was not tested. Never describe a change as verified merely because code was written or looks plausible.

## Routing acceptance tests

Use these cases to check routing behavior when editing this skill. They are behavioral tests, not instructions to print a routing report for every user.

1. **Change a button's specified color:** If editing the code, select `frontend`; exclude `ui-ux-design` because no design choice remains, and exclude unrelated specialists. Inspect the relevant style/token and final diff; a full test suite is unnecessary.
2. **Fix a bug with unknown cause:** Select `debugging` first. Do not pick the implementation layer before evidence supports it. After diagnosis, route to the affected layer and perform a targeted regression check.
3. **Add a persistent feature using an API and UI:** Select `backend-data` and `frontend`; establish the data contract before dependent UI integration. Add `architecture` only if a structural decision is needed, `ui-ux-design` only for new UX decisions, and `security` if the feature changes a trust boundary or access to user/sensitive data. Verify the complete save/load/display flow.
4. **Investigate a slow dashboard with no known bottleneck:** Select investigation/`debugging` and `performance` for evidence gathering. Do not select an optimization implementation in advance. After profiling, route to the layer supported by measurements and compare before/after results when feasible.
5. **Fix an authorization vulnerability:** Select `security`; add `debugging` if the exploit mechanism is unclear, the implementation specialist for the layer enforcing access, and `testing-quality`. Verify unauthorized access is denied and authorized behavior still succeeds. Add `architecture` only if evidence shows a structural trust-boundary flaw.
6. **Answer “Why does this function return a promise?” without changing files:** Inspect the relevant code if available and answer directly. Do not activate implementation, testing, or unrelated skills. Explain uncertainty if the code/context is unavailable.
7. **Check version-specific library behavior:** Inspect the project lockfile and local usage first. If local evidence does not establish whether the target version supports the needed behavior, select `technical-research`; hand the verified finding to the implementation or architecture specialist. If local code/docs answer the question with high confidence, skip external research.
8. **Build a new website/app with no supplied design:** Select `ui-ux-design` and `frontend`. The design handoff must define a product-specific visual direction, page composition, hierarchy, typography/color/spacing roles, key states, and responsive behavior. Frontend must implement—not silently flatten—the handoff, and review the rendered design when tools permit. Judge completion by visual quality and usability as well as working features. Add `testing-quality` when the breadth and risk of the app justify broader behavior verification.
9. **Reported visual overlap in an established UI:** If an icon overlaps placeholder or typed text and the design is already approved, select `debugging` first when the root cause is unknown, then `frontend` to correct the layout and `testing-quality` for responsive regression checks. Do not activate `ui-ux-design` for a mechanical spacing/positioning defect unless investigation uncovers an unresolved design decision. Verify the rendered field at narrow and desktop widths when tools permit.
10. **Persistence failure plus malformed local data:** Select `debugging` if the cause is uncertain, `frontend` for the client behavior, and `testing-quality` for normal and failure-path regression checks. Add `security` only when untrusted data materially crosses a trust boundary or the task specifically requires a security assessment; validate local browser data, but do not mischaracterize localStorage as an authorization boundary. Confirm a failed write cannot produce a false durable-save success message.

The orchestrator succeeds when it chooses a justified path, avoids unnecessary work, adapts to evidence, stays within scope, and accurately reports verified outcomes—not when it activates the most skills or produces the longest plan.
