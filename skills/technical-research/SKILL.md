---
name: technical-research
description: Researches technical questions using authoritative, version-appropriate evidence. Use when decisions depend on current or version-specific documentation, API contracts, library/framework compatibility, release notes, standards, advisories, or genuine technology comparisons. Skip external research when the answer can be established confidently from relevant project code and local documentation.
---

# Technical Research

Produce evidence-backed technical findings, not plausible-sounding guesses. For every consequential claim, identify the source, applicable version/date context, and confidence. Research only when it can materially improve the decision.

## Quick Start: Is Research Necessary?

Ask:

> Can this be answered from the project's code, configuration, lockfile, or local documentation with high confidence and no material risk if wrong?

- **Yes:** answer from project context; do not browse merely to appear thorough.
- **No:** define the exact question and decision, then research using the workflow below.

Examples:

| Question | Research? | Reason |
|---|---|---|
| Why does this route return 404? | Usually no external research | Inspect route definitions, middleware order, and tests first. |
| Does the installed framework version support a particular API option? | Yes, if local source/types do not settle it | Version-specific behavior needs authoritative confirmation. |
| Which new library should be introduced when the current solution works? | Usually no | First establish whether a real problem or requirement exists. |
| Does an external payment API support idempotency keys for this operation? | Yes | The provider's current contract matters. |

## Workflow

1. **Scope the question.** State the exact technical question and the decision the answer will support. Do not widen scope unless evidence reveals a material risk.
2. **Inspect local context.** Check relevant manifests and lockfiles, configured runtime, source usage, types, configuration, tests, and local docs. Record the installed/target version and environment where relevant.
3. **Establish constraints.** Consider platform/OS, language/runtime version, compatibility, license, performance or budget targets, deployment model, browser support, and project conventions only as applicable.
4. **Find authoritative sources.** Start with version-matched official documentation, API references, specifications, or standards. Check official release notes, changelogs, migration guides, and advisories for version-sensitive questions.
5. **Cross-check consequential findings.** For decisions materially affecting architecture, security, data migration, or compatibility, seek corroboration from another relevant authoritative source where practical, such as official docs plus a changelog, source code, type definitions, or tests.
6. **Classify each important finding.** Use `Confirmed`, `Inferred`, `Conflicting`, or `Unknown`, as defined below.
7. **Compare only when a real choice exists.** Include the current implementation or “do nothing” as a baseline when relevant. Do not manufacture alternatives or a comparison table for a choice that has no meaningful competing options.
8. **Report proportionately.** A simple API fact may need one sentence and a source. A migration or architecture decision may need a comparison table, trade-offs, caveats, and a handoff.
9. **Hand off findings.** Give the relevant specialist the verified facts, version context, constraints, implications, and unresolved uncertainties. Do not take over implementation unless explicitly requested and authorized.

## Source Quality and Version Context

Use sources in this general order, adapting to the question:

1. Official documentation, API references, specifications, and standards (for example, RFCs, W3C, or ECMA specifications).
2. Official release notes, changelogs, migration guides, and security advisories.
3. Official source code, type definitions, tests, or issue trackers when published documentation does not settle the behavior.
4. Reputable secondary technical sources for additional context.
5. Community discussions as leads, not as sole evidence for a claim described as officially documented.

Always check that a source matches the product, version, environment, and relevant date. An undated tutorial or “latest” documentation page may not describe the version actually installed. Inspect the project's manifests/lockfiles and verify compatibility rather than assuming that a familiar API exists.

Treat Semantic Versioning as a compatibility signal, not proof. Do not recommend an upgrade merely because a newer release exists. Explain the requirement or problem the upgrade would solve and consider migration cost.

For dependency research, evaluate relevant factors such as compatibility, maintenance/support status, license, release history, and known advisories when those factors can affect the decision. Popularity, downloads, or repository stars alone do not establish technical suitability or security.

## Finding Classification

Label important findings explicitly when uncertainty matters:

- **Confirmed:** Supported by an authoritative source that matches the relevant version/environment, or directly verified in the project.
- **Inferred:** A reasoned conclusion drawn from verified facts but not stated directly by a source. Explain the reasoning.
- **Conflicting:** Relevant sources disagree. Present the conflicting claims, their dates/versions, and which source appears more authoritative or applicable. If the conflict cannot be resolved, leave it unresolved.
- **Unknown:** The claim could not be verified with available evidence. State what is missing and the next check that could resolve it.

Do not label a claim `Confirmed` solely because a search result snippet says it. Do not treat “no vulnerability found in the sources checked” as proof that a dependency is secure.

## Research and Comparison Rules

- Prefer inspecting the actual installed version and current project usage over assuming “latest.”
- Distinguish documented guarantees from observed behavior, maintainer comments, community claims, and your own inference.
- For undocumented behavior, use source code, type definitions, tests, or a focused experiment if accessible; identify that method instead of presenting the behavior as a documented guarantee.
- Compare technologies only against real requirements. Relevant criteria may include API fit, compatibility, license, maintenance, security advisories, performance evidence, bundle/resource impact, migration cost, operational complexity, and lock-in.
- Do not invent benchmark numbers. If a measured comparison matters, benchmark using the project's actual configuration and describe the measurement conditions.
- If external browsing is unavailable, say so and work only from accessible project evidence. Do not fabricate URLs, citations, version numbers, test results, or claims of having read documentation.
- Do not expand a research task into a full security audit, architecture redesign, or implementation. Hand findings to the relevant specialist.

## Reporting Format

### Minimal report for a simple question

Use a proportional one-line answer when sufficient:

> **Confirmed for [product/version]:** [direct answer]. Source: [official source/link].

Include a short caveat only when it changes how the answer should be used.

### Report for a consequential decision

- **Question / decision:** What was investigated and what decision it supports.
- **Finding:** Recommendation with `Confirmed`, `Inferred`, `Conflicting`, or `Unknown` labels for important claims.
- **Evidence:** Source links, version/date context, and corroboration where warranted.
- **Trade-offs:** Only differences that materially affect the decision.
- **Uncertainty:** What could not be verified and what would resolve it.
- **Handoff:** Practical implications for architecture or implementation.

## Examples

### Example 1 — Version-specific API behavior

**Input:** “Does the runtime version used by this project support `AbortSignal.timeout()` with its `fetch` implementation?”

**Process:** Read the runtime constraint and lockfile/configuration, identify the exact runtime version, then consult the version-matched official runtime documentation and release notes. If redirect or retry behavior matters, inspect relevant implementation details or tests instead of assuming browser and server runtimes behave identically.

**Output:**
- **Confirmed:** The version-matched official documentation states whether the API is available for the project's target runtime.
- **Unknown, if applicable:** Behavior in a relevant redirect/retry path remains unverified until its implementation or tests are inspected.
- **Handoff:** Use the API only if the verified runtime supports the required behavior; otherwise, select a compatible alternative or document the limitation.

The example demonstrates the research method. Re-check the actual version and current documentation for each real task; do not reuse example conclusions as permanent facts.

### Example 2 — Comparing libraries for a real requirement

**Input:** “Should we use `date-fns` or `luxon` for a new timezone-aware scheduling feature? We currently use native `Date`.”

**Process:** Confirm whether timezone-aware scheduling is a real requirement, inspect current dependencies and bundle constraints, and consult official documentation for the candidate versions. Compare timezone support, API fit, compatibility, bundle impact measured in the actual build if material, license, maintenance, and migration cost. Include the existing native implementation as a baseline, but do not treat it as viable if it cannot meet the confirmed requirements.

**Output:** A concise comparison of only relevant criteria, an evidence-backed recommendation, source/version context, and any unmeasured bundle or runtime implications. Do not quote stale package sizes or maintenance claims without verifying them against current sources and the project's own build.

### Example 3 — No external research needed

**Input:** “Why does this Express route return 404?”

**Process:** Inspect route declarations, middleware order, mount paths, and relevant tests. These project artifacts can usually answer the question directly.

**Output:** Explain the cause based on the inspected code and identify the relevant location. Do not search the web unless local evidence reveals a version-specific or otherwise unresolved question.

### Example 4 — Conflicting or unknown evidence

**Input:** “Does package X support option Y in our deployed version?”

**Process:** The installed lockfile identifies version 2.x. The current documentation describes version 3.x, while an older tutorial claims the option exists in 2.x. Inspect the version-2 release notes, source, types, or tests. If evidence still does not resolve the conflict, do not infer support from the current documentation.

**Output:**
- **Conflicting:** The current docs show the option, but apply to a different major version; the older tutorial claims support for the installed version but lacks authoritative confirmation.
- **Unknown:** Support in the deployed version remains unverified until matching source, types, tests, or a focused experiment confirm it.
- **Next step:** Inspect the installed package or run a minimal test against the deployed version before recommending the option.

### Example 5 — External access unavailable

**Input:** “Is the latest release of this SDK compatible with our runtime?”

**Process:** Inspect local manifests and lockfiles, but acknowledge that current release information cannot be reached. Do not infer latest release details from memory.

**Output:** State which local facts were confirmed, which compatibility claim remains unknown, and what official release notes or documentation must be checked when external access is available.

### Example 6 — Minimal report

**Input:** “Does the installed package expose method `parseAsync`?”

**Output:** “**Confirmed for package version X.Y:** `parseAsync` is exported, as shown in the version-matched official API reference [link].”

Replace the version and claim only after verifying the actual installed version and source. If either is unavailable, classify the result as `Unknown` instead.

## Specialist Handoffs

- `architecture`: Technology comparisons or findings that affect system structure, service boundaries, or long-term technical decisions.
- `frontend`: UI library, browser API, or client-side SDK facts needed for implementation.
- `backend-data`: Server SDKs, API contracts, database drivers, and persistence behavior.
- `security`: Relevant vulnerability advisories, security specifications, or security-sensitive technology choices.
- `performance`: Documentation and technical findings needed to investigate a measured performance question.
- `debugging`: Research needed after local investigation identifies a specific unknown technical behavior.

Route to a specialist only when the finding affects its work. If sibling-skill invocation is unsupported, provide a concise handoff and do not claim the other skill was invoked.

## Common Failure Modes

- Researching facts already available from project code or local docs.
- Citing a stale tutorial as current, or using documentation for the wrong version.
- Presenting popularity as proof of technical quality.
- Recommending an upgrade without a requirement-linked benefit.
- Treating unverified or conflicting claims as confirmed.
- Fabricating sources, versions, benchmarks, experiments, or access to unavailable systems.
- Expanding research into implementation or specialist audits that were not requested.

## Definition of Done

Research is complete when the decision question has been answered as far as available evidence permits, important claims have appropriate source/version context, uncertainty is labeled honestly, and the next engineering decision has an actionable handoff. A good result may be “Unknown; verify this specific versioned behavior next” rather than an unsupported definitive answer.
