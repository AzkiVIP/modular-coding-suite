---
name: performance
description: Diagnoses and improves software performance by measuring representative workloads, identifying the dominant bottleneck, applying targeted optimizations, and comparing results before and after. Use for latency, responsiveness, throughput, resource consumption, query performance, or capacity concerns. Do not use as a substitute for ordinary debugging when the issue is a functional failure unrelated to performance.
---

# Performance

Diagnose before treating. Every substantial optimization must target an evidenced bottleneck and a meaningful objective. Do not add caching, indexes, memoization, batching, parallelism, or infrastructure merely because they are common best practices.

## Quick Start

When a performance concern arrives:

1. Define what is slow, for whom, under which conditions, and why it matters.
2. Establish an existing target or measure a baseline; state any assumptions.
3. Inspect only the relevant request path, code, configuration, telemetry, and recent changes.
4. Measure the workload with a suitable tool and representative conditions.
5. Identify the dominant cost using supporting evidence; do not optimize the first suspicious-looking code.
6. Implement the smallest change that addresses that cost while preserving correctness and security.
7. Repeat the same measurement under comparable conditions and report the actual result—or clearly state that the improvement remains unverified.

If the environment cannot provide meaningful measurements, say so. A plausible explanation is not a confirmed diagnosis.

## Workflow

- [ ] Define the symptom, affected workflow, impact, and target or baseline.
- [ ] Inspect relevant code, configuration, dependencies, recent changes, and available telemetry.
- [ ] Measure the baseline with a workload-appropriate method.
- [ ] Identify the dominant bottleneck and record evidence that supports or contradicts the hypothesis.
- [ ] Select the smallest effective optimization and explain its trade-offs.
- [ ] Implement within the user's authorized scope, preserving correctness, contracts, access controls, and data integrity.
- [ ] Repeat measurements under comparable conditions and check for regressions.
- [ ] Report verified results, uncertainty, and any checks that could not be performed.
- [ ] Hand off to another specialist only when the evidence justifies it.

Scale the depth to the task. A narrow, already-measured bottleneck may need a focused change; an unexplained slowdown or capacity issue needs investigation first. Do not run a costly load test or produce a full profiling report for an unrelated, low-risk change.

## 1. Establish the Problem

Clarify or infer from available evidence:

- The symptom: loading latency, interaction lag, rendering jank, low throughput, high CPU or memory use, slow queries, or a capacity ceiling.
- The affected workflow and its user or business impact.
- The conditions: data volume, concurrency, cache state, device, network, environment, and whether the issue began after a change.
- An existing service-level objective or target. If none exists, establish a baseline and identify what improvement would matter.

Keep these categories separate:

- **Measured fact:** observed in a profile, trace, log, or benchmark.
- **Hypothesis:** plausible explanation not yet confirmed.
- **Unknown:** missing evidence that could change the decision.

Do not invent an SLO, workload, baseline, or performance result. State assumptions explicitly.

## 2. Inspect and Measure

Read only the relevant request path, component, query, configuration, recent diff, and existing telemetry. Trace end-to-end when necessary; the visually complicated function is not automatically the bottleneck.

Choose the smallest useful set of measurements. Examples include:

- Latency distributions such as p50, p95, and p99; throughput; error rate; and concurrency.
- CPU, memory, allocation, garbage-collection, I/O, or network activity.
- Database query plans, rows scanned versus returned, query frequency, and lock waits.
- Browser loading, resource transfer, main-thread work, rendering, interaction responsiveness, and layout shifts.
- Cost per request or unit of work when cost is part of the stated objective.

Use representative data volume, concurrency, payloads, and cache conditions. Repeat measurements when variability matters. Avoid comparing measurements from materially different environments without explaining the limitation.

## 3. Find the Dominant Bottleneck

Trace candidate stages separately: client, network, application processing, database, external services, and resource contention. For each important hypothesis, identify what supports it and what would disprove it.

If the dominant cause remains unclear, choose the next lowest-cost, most informative investigation—for example, adding a timing span or collecting a focused profile—instead of making speculative changes.

Use diagnostic tools only when they are available and appropriate. Do not claim to have run a profiler, load test, query analysis, or monitoring check unless it actually occurred.

## 4. Choose and Implement an Optimization

For every substantial candidate change, state:

- Which measured bottleneck it targets.
- Why the change is expected to reduce that cost.
- The smallest viable version of the change.
- Relevant trade-offs: latency, throughput, memory, correctness, complexity, operational burden, and cost.

Reject optimizations justified only by phrases such as “best practice,” “more scalable,” or “should be faster.” Avoid unrelated refactors and preserve the user's scope.

Examples of techniques—only when evidence supports them—include reducing unnecessary network round trips, batching repeated data access, changing query structure, adding a justified index, caching suitable data, code splitting, reducing expensive rendering, or addressing measured resource contention.

Before executing potentially expensive diagnostics, migrations, load tests, or database commands against production systems, consider operational impact and use a safe representative environment when feasible. In particular, `EXPLAIN ANALYZE` executes the query; do not run it on production workloads without understanding its impact and authorization.

If an optimization changes authorization, sensitive-data handling, or security boundaries, involve the `security` specialist. Performance improvements must not weaken correctness, data integrity, or access control.

## 5. Verify the Improvement

Repeat the same relevant measurement method under comparable conditions. Compare baseline and result, accounting for variance when material. Report actual values only when observed.

Check for regressions that matter to the change, such as tail latency, throughput, memory consumption, garbage-collection pauses, error rates, data correctness, reliability, and cost. Do not automatically measure every dimension if it is irrelevant; choose based on the optimization's risks.

If no meaningful before-and-after measurement is possible, report the implementation as completed but the performance improvement as **unverified**. Do not turn a successful build or passing unit test into a claim of improved performance.

## Workload-Specific Methods

### Web frontend

- Use browser performance recordings and relevant Core Web Vitals for real web user experiences, not as universal thresholds for every application.
- Separate resource loading, network time, bundle parsing, JavaScript execution, main-thread work, layout, and paint.
- Do not blame excessive re-renders without a profile or other evidence showing that rendering cost is significant.
- Apply lazy loading, code splitting, or virtualization only when the affected resources or rendered content justify them.

### Backend and API

- Trace slow requests across application logic, database access, network calls, and external services.
- Use application timing, distributed traces, or focused instrumentation to identify where time is spent.
- Do not add a cache if an unrelated upstream call, repeated query, or serialized processing stage dominates latency.

### Database

- Inspect a real query plan and representative data volume before changing indexes or query structure.
- Check rows scanned versus returned, query frequency (including N+1 patterns), lock waits, and whether a proposed index is likely to change the plan.
- Consider the storage and write costs of indexes as well as read benefits.
- Use query-analysis tools safely and according to the database engine's documented behavior.

### Concurrency and capacity

- Use representative load tests when needed, tracking throughput, latency distributions, errors, and resource utilization together.
- Distinguish saturation of CPU, connection pools, workers, or other limits from wasteful work per request; the appropriate fix differs.
- Do not infer production capacity from a small or unrealistic benchmark.
- Before increasing pool sizes or concurrency limits, verify downstream capacity and backpressure behavior. More concurrency can move the bottleneck or make overload worse.

### Memory and resources

- Use allocation profiling, heap snapshots, or equivalent evidence instead of guessing about object churn.
- Correlate memory growth and garbage-collection activity with the observed slowdown.
- Determine whether growth is bounded, expected, and within the resource budget. A latency improvement may not be worthwhile if it causes unbounded memory use or unacceptable garbage-collection pauses.

## Specialist Coordination

- `debugging`: involve when an unresolved functional failure is mixed with the slowdown or the cause falls outside performance diagnosis.
- `frontend`: implement a proven client-side improvement.
- `backend-data`: implement a proven API, query, transaction, or data-access improvement.
- `architecture`: evaluate a structural change when evidence shows the existing boundaries or topology are the limitation.
- `testing-quality`: coordinate regression or integration verification when the change is broad or risky.
- `security`: involve when optimization affects authorization, access checks, or sensitive-data handling.

Do not activate all specialists by default. Hand off only when evidence identifies a relevant responsibility. If new findings change the responsible domain, revise the plan before proceeding. If the platform cannot invoke specialist skills, do not claim they were invoked; use only instructions and tools genuinely available.

## Examples

The numerical measurements below are illustrative scenarios, not expected results or claims about a real system. In actual work, use observed measurements only.

### Example 1 — Page loads slowly

**Input:** A dashboard takes about four seconds to become usable.

**Investigation:** A browser trace shows several sequential API calls on the critical path, a large unused JavaScript bundle, and relatively little rendering time.

**Decision:** Address the confirmed critical-path network calls and unnecessary bundle work. Do not change components merely because the page feels slow.

**Verification:** Repeat the trace under comparable conditions, compare time-to-usable behavior, and report the actual measurements. If the result was not measured, leave the improvement unverified.

### Example 2 — A button interaction feels delayed

**Input:** Clicking “Apply Filters” appears to freeze the interface.

**Investigation:** An interaction trace shows the handler itself is short but updating a large table causes a long synchronous render.

**Decision:** Evaluate reducing the amount of rendered work or using virtualization if the measurements and UI behavior justify it. Do not add memoization to the click handler when the handler is not the bottleneck.

**Verification:** Re-record the same interaction and confirm the response time improves without breaking keyboard interaction, filtering behavior, or accessibility.

### Example 3 — A slow API endpoint

**Input:** `/api/orders` has high p95 latency.

**Investigation:** Traces show many repeated database queries for related order items dominate the request time; each individual query is relatively quick.

**Decision:** Evaluate batching or a set-based query using the project's data-access conventions. Do not add response caching before addressing the evidenced query fan-out.

**Verification:** Re-measure the same request workload, inspect query counts and latency, and check response correctness.

### Example 4 — A slow database query

**Input:** A report query takes a long time on a large table.

**Investigation:** A safe, representative query plan shows that the filter expression prevents the expected index path from being used.

**Decision:** Evaluate rewriting the predicate or using a suitable expression index. Do not add arbitrary indexes without evidence that they help the query plan and workload.

**Verification:** Compare query plans and timings using comparable data and conditions. Use `EXPLAIN ANALYZE` only where execution impact is understood and authorized.

### Example 5 — A service slows under concurrent load

**Input:** Errors and latency rise during traffic spikes.

**Investigation:** A representative load test shows a connection pool saturates while per-request application work remains small.

**Decision:** Evaluate pool sizing against database connection limits and other consumers; consider backpressure and bounded queueing. Do not increase concurrency beyond the downstream system's capacity.

**Verification:** Repeat the load test, comparing throughput, p95/p99 latency, errors, and resource saturation. Report the new observed capacity rather than extrapolating beyond the test.

### Example 6 — “Make everything faster”

**Input:** The user gives no target or specific complaint.

**Investigation:** Review available usage and telemetry, or ask which workflows matter most. Establish baselines for a small number of important workflows rather than changing the whole codebase.

**Decision:** Prioritize the workflow where measurements show meaningful impact. Do not apply blanket caching, indexing, or memoization.

**Verification:** Measure the selected workflow before and after the targeted fix and report remaining uninvestigated areas.

### Example 7 — Latency improves but memory and garbage collection worsen

**Input:** A new cache reduces response latency but increases memory use and causes occasional slow garbage-collection pauses.

**Investigation:** Compare latency, memory over time, allocation rate, garbage-collection pauses, cache hit rate, and eviction behavior under the same representative workload. Determine whether memory growth stabilizes or continues without a bound.

**Decision:** Keep the cache only if the latency benefit is meaningful and memory and pause behavior remain within acceptable limits. If growth is unbounded or pauses undermine the target, evaluate bounded capacity, eviction or TTL policies, a smaller cache, or removing the cache. Do not treat lower average latency as a win if tail latency or resource stability becomes unacceptable.

**Verification:** Repeat the same workload long enough to observe cache growth and garbage-collection behavior. Report measured latency and resource trade-offs. If endurance testing was not possible, explicitly leave long-term memory behavior unverified.

## Common Pitfalls Checklist

Before finalizing an optimization, check for these failure patterns:

- [ ] Optimizing code before measuring the relevant bottleneck.
- [ ] Using an unrealistic dataset, concurrency level, cache state, or environment.
- [ ] Reporting average latency while ignoring important tail latency or error-rate changes.
- [ ] Adding caching, indexes, memoization, batching, parallelism, or infrastructure without evidence.
- [ ] Increasing concurrency or connection pools without checking downstream capacity and backpressure.
- [ ] Trading unbounded memory growth, garbage-collection pauses, correctness, or security for a superficial latency win.
- [ ] Comparing before-and-after results measured under different conditions without disclosing the difference.
- [ ] Claiming an improvement without repeating measurements after the change.
- [ ] Using tools that are unavailable or claiming measurements that were never performed.
- [ ] Making unrelated code changes that obscure the effect of the optimization.

If a pitfall applies, correct the plan or explicitly document why the trade-off is acceptable for the measured workload.

## Completion Criteria

A performance task is complete when the relevant bottleneck has been addressed within the authorized scope, appropriate before-and-after evidence has been collected where feasible, important regressions have been considered, and the report distinguishes measured results from assumptions and unverified outcomes.

Do not measure success by the number of optimizations, the sophistication of the tools, or a high skill-generator grade. Measure it by whether the change improves the intended workload without unacceptable trade-offs.
