---
name: testing-quality
description: Designs and runs focused, reliable tests that verify specific behavior, prevent regressions, diagnose test failures, and report evidence honestly. Use when a change needs test strategy, new or updated tests, integration/regression verification, or investigation of flaky tests. Do not require broad testing for every trivial change.
---

# Testing Quality

Testing proves specific claims about behavior with the smallest effective set of reliable tests. It is not merely a matter of making the suite green. A passing suite with incorrect assertions, excessive mocking, or suppressed failures is not meaningful evidence.

## Quick Start

Before writing a test, answer three questions from the actual requirement and project context:

1. **What behavior must be proven?** Test the contract, not incidental implementation details.
2. **What is the smallest test scope that can prove it?** Choose unit, integration, end-to-end, contract, regression, or another relevant method.
3. **What evidence will demonstrate success?** Define the observable outcome and the check that establishes it.

Then inspect the existing test framework and conventions, write focused tests, run them using the project's real commands, and report the actual results.

Example: for a discount-calculation bug, prove that the discount never exceeds the required cap across relevant inputs. A focused unit test is sufficient if the logic is isolated; an end-to-end test is not automatically needed. When feasible, confirm the regression test fails against the buggy implementation and passes after the fix.

## Workflow

- [ ] Identify the behavior, acceptance criteria, and important failure conditions.
- [ ] Inspect nearby tests, framework, conventions, available commands, and relevant dependency versions.
- [ ] Scale inspection to the change's scope and risk.
- [ ] Select the smallest effective test strategy.
- [ ] Write tests with clear names, meaningful assertions, realistic data, and appropriate isolation.
- [ ] Run the relevant tests using actual project commands.
- [ ] Diagnose failures instead of weakening tests to get a pass.
- [ ] Re-run checks needed to verify the fix and investigate flakiness when suspected.
- [ ] Report what ran, what passed or failed, what was skipped, and what remains unverified.

## 1. Understand What Must Be Verified

- Extract expected behavior, acceptance criteria, and failure modes that matter.
- Read existing tests near the change and inspect relevant test configuration (for example, project scripts, test-runner config, and CI workflows).
- Identify affected components, callers, and external boundaries such as databases, APIs, filesystems, queues, or services.
- Match test scope to risk: a small pure-function fix usually needs focused unit tests; a shared authentication or data-contract change can require broader verification.
- If ambiguity changes what “correct” means, state a clear assumption or ask a focused question before encoding it in a test.

## 2. Select the Test Strategy

| Situation | Suitable approach |
|---|---|
| Isolated logic, algorithms, boundary math | Unit test |
| Behavior crosses a real boundary (database, API, file, service) | Integration test |
| Critical user journey through the integrated system | A small number of end-to-end tests |
| Components must agree on an interface | Contract test |
| A known defect must not silently return | Regression test |
| Throughput, latency, or workload is the objective | Coordinate with `performance` |
| Security controls or vulnerability behavior must be verified | Coordinate with `security` |

Treat the test pyramid as a heuristic, not a fixed ratio. Let architecture, risk, and failure impact determine the test portfolio. Do not apply every category to every change.

## 3. Write Reliable Tests

- Name tests after behavior and expected outcomes, such as `rejects_withdrawal_when_balance_is_insufficient`.
- Assert on real outcomes: return values, state changes, emitted events, persisted records, or observable UI behavior. “No exception was thrown” is rarely sufficient by itself.
- Cover the normal case and relevant boundaries, invalid inputs, and realistic failure conditions. Choose edge cases appropriate to the domain rather than mechanically testing every imaginable value.
- Mock at genuine boundaries such as network, time, randomness, or third-party services. Do not mock the behavior under test so extensively that the test proves only that mocks were called.
- Use realistic, controlled fixtures. Keep tests isolated and order-independent, and avoid uncontrolled shared mutable state.
- Prefer condition- or event-based waits over arbitrary `sleep()` calls.
- Do not add redundant tests just to increase coverage percentages. Each test should provide useful evidence that other tests do not already provide.

### Good test vs. bad test

**Scenario:** a service should reject a transfer when the account has insufficient funds.

Bad test — verifies only that a mock was called, without checking the business outcome:

```javascript
it("handles transfer", async () => {
  await service.transfer({ amount: 500 });
  expect(accountRepository.save).toHaveBeenCalled();
});
```

This test may pass even if the transfer incorrectly succeeds or the balance becomes negative. Its setup also does not establish the account's initial balance or the expected failure behavior.

Better test — verifies the relevant contract and absence of an unintended side effect:

```javascript
it("rejects a transfer when funds are insufficient", async () => {
  accountRepository.findById.mockResolvedValue({ id: "a1", balance: 100 });

  await expect(
    service.transfer({ accountId: "a1", amount: 500 })
  ).rejects.toThrow("Insufficient funds");

  expect(accountRepository.save).not.toHaveBeenCalled();
});
```

This is illustrative pseudocode: adapt function signatures, error types, and test conventions to the actual project. A unit test like this should be complemented by an integration test if database transaction behavior is part of the contract being verified.

## 4. Diagnose Test Failures and Flakiness

When a test fails, investigate before changing it:

1. Read the assertion and compare actual with expected values.
2. Reproduce the failure in isolation, then run the surrounding test file or suite as needed.
3. Classify the likely cause:
   - Product defect: fix the implementation, not the valid assertion.
   - Incorrect expectation: change the test only when the requirement or original expectation is demonstrably wrong, and explain why.
   - Stale fixture or test data: update it to reflect the intended contract.
   - Environment or dependency issue: investigate the environment instead of misattributing the failure to product code.
   - Timing, ordering, or nondeterminism: correct synchronization or isolation rather than hiding the failure.
   - Unknown root cause: hand off to `debugging` instead of guessing with code changes.
4. Never loosen an assertion, delete a meaningful test, inflate timeouts, or add blind retries merely to get a green result.
5. If flakiness is suspected, repeat the relevant test using the framework's repeat facility or a justified loop and report the observed failure rate. One passing run does not prove the issue is fixed.
6. Preserve the original intent of existing tests unless that intent is shown to be wrong or the requirement has changed.

## 5. Regression and Integration Risk

- For a reported bug, add a focused regression test when practical. Where the old implementation is available and the test can be run safely, confirm it fails before the fix and passes after it.
- For cross-boundary changes, verify actual contracts: request and response shapes, schemas, status codes, error formats, and compatibility with affected consumers.
- Expand verification when changes affect shared components, persistence, authorization, migrations, or behavior beyond the immediate caller.
- For destructive or security-sensitive changes, coordinate with the relevant specialist and use disposable or isolated data. Verify both the protected failure path and legitimate behavior.
- Do not claim broad regression safety based only on a narrow test that does not exercise affected boundaries.

## 6. Test Data and Environments

- Use isolated or disposable data and environments, such as seeded fixtures, temporary databases, test containers, or in-memory stores where appropriate. Do not run destructive tests against production data or live third-party systems without explicit authorization.
- Use fakes or stubs for speed and determinism, but include a real integration check when the actual dependency's behavior is material to correctness.
- Make setup and teardown explicit, including cleanup when a test fails.
- If a failure may be environmental (network, container readiness, port conflict, credentials, or dependency availability), investigate that possibility before concluding the application is defective.
- Do not introduce a testing framework casually. If the repository has no established test setup, assess whether existing tools or the language standard library can verify the behavior; add a dependency only when the risk and benefit justify it, and explain the trade-off.

## 7. Run and Report

- Use the project's actual test scripts, commands, and CI conventions. Do not invent a command and imply it was the project's established workflow.
- Run focused tests for localized changes; broaden to integration, regression, or end-to-end checks when the scope and risk justify them.
- Report which commands actually ran, pass/fail/skip outcomes, specific failures, whether the requested behavior is covered, and any environmental constraints or unverified cases.
- A successful build, a high coverage percentage, or a passing suite is only a partial signal. None proves the specific requested behavior unless the tests assert it.
- Never report tests as passed unless they were executed and passed.

## 8. Coordinate With Other Specialists

- `debugging`: unexplained failures and root-cause analysis beyond diagnosing the test itself.
- `frontend` / `backend-data`: implementing the behavior under test when the implementation is missing or incorrect.
- `architecture`: structural changes that materially affect testability or system boundaries.
- `security`: vulnerability verification and access-control correctness.
- `performance`: load, throughput, latency, and profiling work.

Hand off when warranted rather than duplicating another specialist's full workflow. `testing-quality` verifies behavior; it should not redesign the architecture or modify production code beyond what is needed for a diagnosed and authorized fix.

## Required Acceptance Scenarios

### 1. Small function change

**Situation:** a pure function receives a localized logic fix.

**Approach:** test the changed behavior, relevant boundary values, and the specific defect. Do not run the entire suite or add integration/end-to-end tests without a reason.

**Sufficient evidence:** focused tests pass, the changed logic's relevant edge cases are covered, and related module tests pass when run.

### 2. Cross-component feature

**Situation:** a feature spans an API and database.

**Approach:** test the request/response contract, persistence, and relevant error paths. Use an actual test database or representative dependency when its behavior matters. Add an end-to-end test only if a full user journey is materially at risk.

**Sufficient evidence:** relevant integration checks pass, the contract matches the specification, and affected consumers are not known to be broken.

### 3. Regression test for a reported bug

**Situation:** a defect has been observed in a report or issue.

**Approach:** write the smallest test that reproduces the defect. Where feasible, demonstrate failure with the buggy implementation and success after the fix.

**Sufficient evidence:** the test asserts the defective behavior directly and passes against the fixed code. If the old implementation cannot safely be tested, state that limitation.

### 4. Flaky test

**Situation:** unchanged code passes and fails nondeterministically.

**Approach:** repeat the relevant test in isolation and in the wider suite as needed; inspect timing, ordering, shared state, external dependencies, and synchronization. Avoid blind retries, arbitrary timeout increases, or silently skipping the test.

**Sufficient evidence:** a plausible root cause has been identified, a deterministic correction is applied, and repeated runs provide evidence of improved stability. Report the number and conditions of the runs.

### 5. API/database integration

**Situation:** compatibility with a real API or database behavior matters.

**Approach:** verify request/response shapes, serialization, status and error behavior, and persistence using a real or representative test dependency. Mocks may support unit tests but must not be the sole evidence of a contract that depends on actual integration behavior.

**Sufficient evidence:** a real or high-fidelity integration/contract check passes, with mocks treated as supplementary evidence.

### 6. Security-sensitive change

**Situation:** authentication, authorization, or access to sensitive data changes.

**Approach:** coordinate with `security`. Verify that an unauthorized or malformed request is denied and that legitimate access still works. Use isolated test data and do not exercise destructive checks against production resources.

**Sufficient evidence:** the relevant negative and positive cases pass, plus the required regression/integration checks. State any security behavior that was not verified.

### 7. Search/input icon and text overlap

**Situation:** a leading or trailing icon, placeholder, typed value, or clear control overlaps in a search/input field.

**Approach:** reproduce at representative narrow and desktop widths, including focus and populated states. Inspect the rendered box and relevant computed layout (padding, gap, icon position/width, line height, responsive rules). Verify long placeholder/value behavior and keyboard/focus visibility. Do not rely only on a screenshot at one width or on a style token existing.
Check the final computed padding and cascade order too: a later equal-specificity `input[type="search"]` shorthand can overwrite a preceding component rule such as `.field--search input { padding-left: 36px; }`. Assert the actual rendered separation, not just the presence of the intended declaration.

**Sufficient evidence:** the icon and text have distinct usable space and do not overlap in checked states/viewports; any untested browser or viewport is named explicitly. When browser rendering is unavailable, record source-level checks as limited evidence rather than a passed visual test.

### 8. Persistence write fails

**Situation:** a create/edit/delete action can show success even though localStorage or another persistence write throws/fails.

**Approach:** use a controllable storage adapter/stub or safe browser test to make the write fail. Assert the actual user-visible contract: no false durable-save success, visible state matches rollback/unsaved-state policy, and the ordinary success path still works. Include reload verification using real browser storage when practical.

**Sufficient evidence:** the failure path is asserted directly, legitimate saves still work, and persistence tested via a stub is clearly distinguished from real storage/reload verification.

### 9. Malformed persisted values and keyboard focus

**Situation:** records restored from localStorage may contain invalid status/priority values, or an action re-renders the list and loses keyboard focus.

**Approach:** test allowed enum values plus unknown and inherited-name tokens (such as `toString`, `constructor`, and `__proto__`). Confirm invalid records are rejected or normalized and do not break valid records. After create/edit/delete/status changes, verify focus remains on a sensible control and keyboard users can continue the workflow.

**Sufficient evidence:** valid records and controls continue to work, malformed values have a defined outcome, and focus behavior is verified in a browser where feasible. Do not claim visual or keyboard verification from a syntax check alone.

### 10. Repository without an established test framework

**Situation:** a small or older project has no obvious test harness.

**Approach:** inspect available language tools and existing scripts. Determine whether a focused check can be written using existing tooling or the standard library. Recommend a new testing dependency only when it offers a meaningful benefit relative to project size and risk.

**Sufficient evidence:** the chosen check runs reproducibly, or the report explains why automated verification was not feasible and what was checked instead.

## Common Pitfalls

- Tests that only confirm a mock was called without asserting the actual behavior.
- Tests that pass regardless of whether the target defect is present.
- Changing assertions or deleting tests simply to obtain a green suite.
- Adding arbitrary sleeps or blind retries to hide timing problems.
- Mocking the dependency whose real behavior is the subject of the test.
- Running a full suite for every trivial change while overlooking high-risk integration boundaries.
- Using production data for destructive testing.
- Claiming coverage or build success proves the requested behavior.
- Reporting unexecuted tests as passed.
- Treating a syntax/lint check as proof that an icon and text do not overlap in the rendered interface.
- Testing only successful storage writes while never asserting the error path.
- Verifying enum values by exercising only ordinary values, not malformed or inherited-property names.

## Completion Criteria

Testing work is complete when the selected tests provide proportionate evidence for the requested behavior, relevant failures have been investigated, actual results are reported honestly, and significant unverified risks are disclosed. The objective is reliable evidence—not the largest test count or a universally fixed testing ratio.
