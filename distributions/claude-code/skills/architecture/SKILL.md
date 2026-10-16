---
name: architecture
description: Makes evidence-based software architecture decisions for new and existing projects by inspecting relevant code and documentation, establishing requirements and quality attributes, comparing realistic trade-offs, and producing actionable decisions about component boundaries, interfaces, and architecture records. Use when designing a new system structure, evaluating a significant structural change, choosing between consequential technical approaches, planning a migration or refactor, or when architecture documentation may be stale. Do not use for routine feature implementation, isolated bug fixes, UI styling, or test writing that does not involve structural decisions.
---

# Architecture

## Core Principle

Architecture decisions can be expensive to reverse. **Inspect first, decide second, document third.** The simplest architecture that satisfies verified requirements is preferable to a more complex design justified only by speculative future needs.

## When to Activate

**Activate when:**
- Starting structural design for a new project or major subsystem.
- A change affects module boundaries, data flow, service topology, dependency direction, or public/internal contracts.
- Choosing between consequential approaches, such as monolith vs. services, synchronous vs. asynchronous processing, SQL vs. NoSQL, or REST vs. events.
- Planning a migration, substantial refactor, or framework/platform change.
- Existing architecture documents appear stale or contradict the implementation.
- The user explicitly asks how a system or subsystem should be structured.

**Do not activate when:**
- Implementing a feature within established boundaries and conventions.
- Fixing an isolated bug whose cause and scope are local.
- Making a typo, copy, styling, or other cosmetic change.
- Writing tests for existing behavior without changing structural decisions.
- Tuning one function or query unless evidence reveals a structural issue.

When uncertain, ask whether the task changes a boundary, contract, dependency, or consequential technical decision. If it does not, leave the task to the relevant implementation specialist.

## Coordination with Other Specialists

This skill decides **structure and contracts**, not detailed implementation.

| Specialist | Coordination rule |
|---|---|
| `frontend` | Define component/module boundaries and relevant contracts consumed by the UI. Frontend owns UI implementation, state details, and styling. |
| `backend-data` | Define service boundaries, data ownership, flows, and interfaces. Backend owns endpoint implementation, business logic, and database query details. |
| `debugging` | If diagnosis reveals a structural root cause, use this skill to decide the architectural correction. Routine diagnosis remains with debugging. |
| `security` | Identify relevant trust boundaries and security implications. Detailed threat analysis and vulnerability remediation belong to security. |
| `performance` | Define performance requirements and architectural trade-offs. Profiling and implementation-level optimization belong to performance and the affected implementation specialist. |
| `technical-research` | When available, delegate research requiring current external information, such as library/version compatibility or vendor comparisons. If unavailable, use accessible authoritative sources or state the research limitation. |
| `testing-quality` | Define testability constraints and where contract or integration tests are needed. Detailed test design and execution belong to testing-quality. |

Only involve specialists that have a justified role. Do not claim a specialist was invoked if the host platform cannot invoke it or its instructions are inaccessible.

## Workflow

1. Inspect relevant reality: source code, dependencies, configuration, interfaces, and architecture documents.
2. Establish requirements, quality attributes, constraints, and acceptance criteria.
3. Identify realistic alternatives when the decision has meaningful alternatives.
4. Compare relevant trade-offs in proportion to the significance of the decision.
5. Choose the simplest option that satisfies the verified requirements.
6. Define concrete boundaries, contracts, and data flows as needed.
7. Record the decision and update architecture documentation when justified.
8. Hand implementation specialists an actionable specification and unresolved questions.

Scale the process to the decision. A small, reversible change does not require a formal architecture study, multiple diagrams, or an ADR. A consequential change should not skip requirements, alternatives, risks, or compatibility checks merely to move faster.

## Step 1: Inspect Reality

For an **existing project**, inspect only the context relevant to the decision, expanding as needed:
- Read the actual directory structure; do not infer the architecture from folder names alone.
- Open relevant source files and trace important callers, callees, interfaces, and data flows.
- Inspect dependency manifests and lockfiles for actual versions and dependencies in use.
- Check configuration, deployment definitions, CI/CD, and infrastructure-as-code when they affect the decision.
- Read existing architecture documents, diagrams, and ADRs, then verify key claims against code. Call out contradictions instead of silently trusting stale documents.
- Identify current module boundaries, ownership of data, and dependency direction.

For a **new project**, inspect any existing scaffold and determine the stated product constraints, technology requirements, team capabilities, delivery timeline, hosting restrictions, and compliance obligations.

Do not claim to have inspected files that were not accessible. When evidence is missing, state the limitation and distinguish facts from assumptions.

## Step 2: Establish Requirements Before Deciding

For consequential decisions, establish:

1. **Functional requirements:** specific capabilities the system must provide.
2. **Quality attributes:** observable or testable expectations where possible, not vague goals such as “fast” or “scalable.” Example: “At the expected peak load, p95 API latency should remain below 300 ms.” Do not invent a target and present it as user-provided; propose it as a target for confirmation when necessary.
3. **Constraints:** existing stack, team size and skills, budget, timeline, deployment, compliance, compatibility, and operating capacity.
4. **Acceptance criteria:** evidence that the architecture meets the important requirements.

If a missing requirement could materially change a costly or hard-to-reverse decision, ask a focused question before committing. If it is low-impact and reversible, proceed with an explicit assumption and explain how it could affect the decision.

## Step 3: Generate Realistic Alternatives

For consequential decisions, compare two or three genuinely viable options, including the smallest-change or no-structural-change baseline when applicable. Base the options on inspected evidence and stated constraints, not generic best-practice lists.

For low-stakes, easily reversible decisions, following an established project convention may be sufficient; do not invent alternatives just to satisfy a checklist.

## Step 4: Compare Trade-offs

Discuss only the factors material to the decision:
- **Maintainability:** cognitive load, coupling, and change propagation.
- **Performance:** fit against the stated workload and quality targets.
- **Reliability:** failure modes, blast radius, and recovery.
- **Security:** trust boundaries and attack-surface changes.
- **Operational complexity:** deployment, monitoring, incident response, and added moving parts.
- **Cost:** infrastructure, licensing, and engineering/maintenance time.
- **Migration risk:** data migration, downtime, rollback, compatibility, and blast radius.

Use a compact trade-off table when multiple viable options have meaningful differences. Avoid false precision or invented scores when reliable measurements are unavailable.

## Step 5: Choose the Simplest Sufficient Option

Prefer the least complex option that meets the established requirements and avoids known, unacceptable limitations. Do not add technology or abstractions solely because:
- “It is a best practice,” without a relevant requirement.
- “We might need it later,” without credible evidence.
- “It is more scalable,” when the actual workload does not justify the added complexity.
- A framework or architecture style is popular or familiar.

State significant things deliberately not included and why, such as “No message queue: the current requirements do not need asynchronous decoupling or burst buffering.” Do not claim a design is future-proof; explain what change would justify revisiting it.

## Step 6: Define Boundaries and Contracts

When relevant, specify concretely:
- Component/module responsibilities that are distinct and non-overlapping.
- Allowed dependency direction and important forbidden dependencies.
- Data ownership and flow, including synchronous/asynchronous behavior where relevant.
- Interfaces and API contracts: inputs, outputs, error semantics, authorization assumptions, and compatibility expectations as applicable.
- Migration and rollout constraints if changing an existing system.

Be concrete enough that implementation specialists do not need to invent missing requirements. If an important detail remains undecided, label it as an open question rather than silently assuming an answer.

## Step 7: Document When Justified

Match documentation effort to decision significance:
- **Trivial and reversible:** a brief explanation is sufficient; no formal document is needed.
- **Moderate or non-obvious:** create a concise ADR when it will help future maintenance.
- **Significant:** for example, a new service boundary, a data-model redesign, or a platform shift, create/update an ADR and the relevant architecture map or C4 diagram. Use only the diagram levels that clarify the decision.

Use an existing repository convention if one exists. Otherwise, `docs/adr/` or `docs/architecture/` may be suitable locations; do not create duplicate documentation structures unnecessarily.

### ADR Template

```markdown
# ADR-NNN: <decision title>

## Status
Proposed | Accepted | Superseded by ADR-XXX

## Context
Requirement/problem, constraints, and evidence inspected.

## Decision
What was chosen and why.

## Alternatives Considered
- Option A — rejected because...
- Option B — rejected because...

## Consequences
Benefits, costs, risks, and follow-up work.
```

Update existing architecture documents when a meaningful decision changes them. Do not leave contradictory documents without clearly marking what is outdated and what replaces it.

## Step 8: Produce an Actionable Handoff

For implementation specialists, provide only what is relevant:
- The decision and concise rationale.
- Concrete module responsibilities, contracts, dependencies, and data flow.
- Explicit scope boundaries and non-goals.
- Open assumptions, risks, and unresolved decisions.
- Required compatibility, migration, or verification checks.

Architecture should not take over detailed implementation or produce lengthy documentation that does not help the decision or handoff.

## Tools and Methods (Use Selectively)

- **C4 Model:** communicate system context, containers, and components when diagrams improve understanding. Context and container diagrams are often enough; use component-level detail only when useful.
- **Architectural Decision Records (ADRs):** preserve the rationale and consequences of consequential or non-obvious decisions.
- **Quality attribute scenarios:** translate vague goals into a stimulus, operating environment, expected response, and measure.
- **Dependency analysis:** inspect actual imports, calls, and dependency rules before judging module boundaries.
- **Trade-off tables:** compare options when multiple viable choices differ on important factors.
- **Official documentation and compatibility checks:** verify framework/library versions, APIs, and migration concerns when they affect the decision.

Use only the methods that fit the decision. A configuration adjustment does not need a C4 diagram; a new service boundary likely warrants an ADR and an updated system/container view.

## Examples

### Example 1 — Existing Project: Should a Monolith Become Microservices?

**Input**

> “Should we split the monolith into microservices to scale better?”

**Inspected evidence (illustrative example, not a default assumption)**
- The project is a Django monolith of approximately 12,000 lines of code.
- It uses one PostgreSQL database and deploys as one container.
- Measured peak traffic is approximately 50 requests per second.
- The team reports that coordinated releases are painful. Further discussion confirms that release coordination—not request capacity—is the main problem.

**Process and trade-off**

| Option | Benefit | Cost/risk | Assessment |
|---|---|---|---|
| A. Keep the current structure and improve CI/CD and feature flags | Lowest structural change; may reduce release friction quickly | Does not resolve unclear internal module ownership by itself | Useful immediate measure |
| B. Modularize inside the monolith and enforce boundaries | Clarifies ownership while retaining one deployable system and one data platform | Requires boundary cleanup and dependency checks | Best fit for the confirmed problem |
| C. Extract a high-churn module into a separate service | Independent deployment for that module | Adds service deployment, network failure modes, monitoring, and distributed debugging | Reconsider only if internal modularization does not resolve the concrete ownership/deployment conflict |

**Concrete output**

- **Decision:** Choose Option B first. Establish module boundaries and ownership inside the existing monolith. Improve CI/CD and feature-flag practices where they directly reduce release coordination pain.
- **Why:** The evidence identifies release coupling as the problem; current traffic does not establish a need to split services for capacity.
- **Boundaries:** Define which module owns each domain capability, which modules may import from others, and the public interfaces between modules. Enforce these rules with available dependency checks where practical.
- **Non-goals:** Do not introduce Kubernetes, a service mesh, or a message queue without a separate requirement that justifies their operational cost.
- **Documentation:** Record the decision in an ADR and update the module map. Do not create a new C4 container diagram solely for this change because the deployment remains one container; update a component-level view if it clarifies the new boundaries.
- **Revisit trigger:** Consider extracting a specific module only if measurable release/ownership friction remains after internal boundaries and delivery practices are improved, or if new reliability/scaling requirements justify separation.

### Example 2 — New Project: Internal Equipment-Maintenance Tool

**Input**

> “We're building a new internal tool for 20 users to track equipment maintenance.”

No technology stack, hosting constraints, detailed compliance requirements, or delivery timeline have been provided. Do not treat these missing details as confirmed requirements.

**Requirements and assumptions**

- **Confirmed:** Approximately 20 users need an internal way to track equipment maintenance.
- **Working assumption:** The first release needs basic equipment records, maintenance history, due dates, and a way to record completed work. Confirm this scope with the user before implementation.
- **Working assumption:** Load is low and the system does not require independent scaling of separate domains. This follows from the stated user count only as an initial assumption, not a measured capacity guarantee.
- **Unknown:** Preferred stack, identity/authentication environment, hosting, backup/retention rules, and delivery deadline. Ask about these if they change a consequential decision; otherwise record assumptions for a reversible initial proposal.

**Alternatives considered**

| Option | Benefit | Cost/risk | Assessment |
|---|---|---|---|
| A. Single application with clear internal modules and a relational database | Simple deployment and transactions; familiar CRUD model; low operational overhead | Components share one deployment lifecycle | Strong initial fit for the stated scale and team of two developers |
| B. Separate frontend and backend deployments with a relational database | Independent UI/API deployment and clear API boundary | More configuration, deployment coordination, and contract maintenance | Consider if the existing environment or team workflow requires this separation |
| C. Multiple independently deployed services | Independent scaling and release boundaries | Higher operational and debugging overhead, distributed failure modes, and more deployment work | Not justified by the currently stated requirements |

**Concrete output**

- **Decision (provisional):** Start with a modular monolith backed by a relational database, using a technology stack already supported by the team or required by the organization. Do not select a specific framework until the stack constraint is known.
- **Initial modules:** `Equipment` owns equipment identity and metadata; `Maintenance` owns maintenance schedules and maintenance history; `Users/Access` integrates with the organization's existing authentication approach if one is available. Keep boundaries logical rather than deploying each module separately.
- **Data flow:** The UI submits validated commands to the application layer; the application layer enforces authorization and business rules, then persists equipment and maintenance records through the data-access layer. Maintenance history must remain associated with the relevant equipment record.
- **Persistence:** Use database constraints and transactions for relationships and updates that must remain consistent. Select exact tables and fields after confirming required workflows and reporting needs.
- **Security and operations:** Restrict access to authorized internal users, protect credentials and secrets, and establish backups and restore expectations based on the organization's requirements. Do not assume that “internal” means access controls are unnecessary.
- **Non-goals:** No microservices, message broker, distributed cache, or additional orchestration platform for the initial release without a specific requirement that warrants the complexity.
- **Acceptance criteria:** Authorized users can create and update equipment records, record and review maintenance history, identify upcoming/overdue maintenance, and retrieve persisted data after signing in again. Permission boundaries, backup expectations, and deployment constraints must be confirmed before production use.
- **Documentation:** Create a concise architecture overview and an ADR if the choice has meaningful long-term consequences or departs from organizational conventions. Add a C4 context/container diagram only if it clarifies users, the application, and external identity or infrastructure dependencies.
- **Revisit triggers:** Reassess the design if confirmed requirements introduce strict audit/compliance controls, multiple teams needing independent releases, significant load, high availability targets, or integrations that materially change system boundaries.

This is a provisional architecture based on explicitly identified assumptions. It must be adjusted if the user confirms different workflows, constraints, stack requirements, or operational needs.

## Completion Criteria

An architecture task is complete when:
- The decision addresses the actual requirements and constraints.
- Important claims about an existing project are supported by inspected evidence.
- Relevant alternatives and consequential trade-offs have been considered without forced analysis for trivial decisions.
- Component responsibilities and interfaces are concrete enough for implementation to proceed without invented requirements.
- Significant assumptions, risks, exclusions, and revisit triggers are explicit.
- Documentation is updated when its expected long-term value justifies it.
- The output distinguishes verified facts from assumptions and does not claim checks that were not performed.
