---
name: backend-data
description: Implements backend business logic, APIs, input validation, database operations, transactions, and schema/data migrations with emphasis on correctness, data integrity, authorization, compatibility, and safe failure handling. Use when adding or changing server-side behavior, API endpoints, persistence, resource access, or database migrations. Do not use for frontend-only work or isolated architectural decisions that do not require backend implementation.
---

# Backend & Data

Implement backend functionality that works beyond the happy path. Preserve data integrity, correct authorization, API compatibility, and safe failure handling without adding complexity the project does not need.

## Quick Start

Before implementing, establish from relevant project evidence:

1. **What exists?** Inspect the affected route/controller, business logic, model/schema, migrations, dependency versions, and tests as needed.
2. **What are the real contracts?** Check request/response shapes, status codes, error formats, authentication, and existing consumers. Do not invent conventions.
3. **What must remain true?** Identify data invariants, ownership rules, transaction boundaries, concurrent operations, and compatibility requirements.
4. **What is the smallest correct change?** Avoid new dependencies, versioning, pagination, or elaborate migration machinery unless the actual requirements justify them.

Scale inspection and verification to the scope and risk. A localized validation fix does not require a full repository audit; a schema migration or cross-service contract change does.

## Workflow

- [ ] Inspect relevant code, schema, migrations, contracts, dependency versions, and tests.
- [ ] Identify invariants, resource ownership, affected consumers, and failure modes.
- [ ] Implement validation, business logic, and persistence within existing conventions.
- [ ] Apply transaction or concurrency controls where the operation requires them.
- [ ] Enforce authorization for the specific resource and operation.
- [ ] Preserve consistent, safe error behavior and API contracts.
- [ ] Plan compatibility and recovery for risky migrations.
- [ ] Run relevant checks and report what was actually verified.

## 1. Inspect Before Implementing

- Inspect only the relevant context for small changes; trace dependencies, consumers, and migration history for changes with broader impact.
- Match the project's existing architecture layering and framework idioms rather than introducing a new pattern unilaterally.
- Never invent endpoints, fields, model names, library APIs, or versions. Check manifests, lockfiles, source contracts, and official documentation when needed.
- If a requirement or contract remains materially uncertain, ask a focused question or state an explicit assumption rather than silently inventing behavior.

## 2. Implement APIs and Business Logic

- Follow existing route, naming, validation, response, and error-handling conventions.
- Validate untrusted input server-side, including type, format, length, range, required fields, and allowed fields as appropriate.
- Explicitly allow only fields the caller is permitted to change. Do not pass an unfiltered request body directly into a model create/update operation.
- Use HTTP methods and status codes appropriate to the operation and consistent with the actual API contract. Do not force an example's status code if the project's documented convention differs without good reason.
- Keep error responses useful but do not expose stack traces, secrets, sensitive payloads, or unnecessary internal details.
- Keep business rules explicit and separate concerns according to the existing project design. Do not expose internal database structures as public API responses when a suitable contract is needed.

### Example: Prevent mass assignment

Illustrative example for an Express-style endpoint using Prisma-style data access. Adapt it to the project's actual framework, validator, types, ORM, and response conventions; do not introduce these tools solely to copy the example.

**Before — unsafe:**

```ts
app.patch("/me/profile", async (req, res, next) => {
  try {
    await prisma.user.update({
      where: { id: req.user.id },
      data: req.body,
    });
    res.sendStatus(204);
  } catch (error) {
    next(error);
  }
});
```

The raw request body may include fields that this endpoint should not permit the user to change, such as `role` or other server-controlled properties.

**After — allow-list and validate:**

```ts
app.patch("/me/profile", async (req, res, next) => {
  try {
    const body = req.body;
    const allowed = new Set(["displayName", "bio"]);

    if (!body || typeof body !== "object" || Array.isArray(body)) {
      return res.status(400).json({ error: "Invalid request body" });
    }

    if (Object.keys(body).some((key) => !allowed.has(key))) {
      return res.status(400).json({ error: "Unexpected field" });
    }

    const { displayName, bio } = body;

    if (
      (displayName !== undefined &&
        (typeof displayName !== "string" || displayName.trim().length > 80)) ||
      (bio !== undefined && (typeof bio !== "string" || bio.length > 500))
    ) {
      return res.status(400).json({ error: "Invalid profile fields" });
    }

    const data: { displayName?: string; bio?: string } = {};
    if (displayName !== undefined) data.displayName = displayName.trim();
    if (bio !== undefined) data.bio = bio;

    await prisma.user.update({
      where: { id: req.user.id },
      data,
    });

    return res.sendStatus(204);
  } catch (error) {
    return next(error);
  }
});
```

The example demonstrates explicit field control and basic validation; production code must also follow the project's runtime validation, typing, error-handling, and authorization conventions. Do not assume this snippet is a drop-in replacement for every stack.

## 3. Protect Data Integrity and Queries

- Enforce important invariants using suitable database constraints (such as primary keys, foreign keys, uniqueness, required values, and checks) in addition to application validation where appropriate.
- Use a transaction when multiple database writes must succeed or fail together. Decide what happens on partial failure: rollback, a compensating action, or a deliberate recovery process.
- Consider concurrent requests where they can race; use an appropriate mechanism such as a unique constraint, transaction isolation, locking, or optimistic concurrency based on the database and operation.
- Use parameterized queries or the ORM/driver's safe query APIs. Never concatenate untrusted values into SQL.
- For dynamic query structure—such as column names or sort direction—use an explicit allow-list; ordinary value parameterization does not make arbitrary SQL identifiers safe.
- Add indexes, filtering, pagination, and resource limits when justified by expected data volume, query patterns, or abuse risk. Apply sane defaults and maximums when an unbounded endpoint could become unsafe or impractical.

## 4. Enforce Access and Operation Semantics

- Every resource operation must verify that the caller may access that specific resource and perform that specific action. Authentication alone is not authorization; a client-supplied ID does not prove ownership.
- Reuse the existing authentication/session mechanism instead of creating a parallel one.
- Protect server-controlled fields and prevent users from changing roles, ownership, permissions, or other privileged properties through ordinary update endpoints.
- Keep safe HTTP methods such as `GET` and `HEAD` free of intended state-changing side effects.
- Do not automatically retry non-idempotent operations after uncertain outcomes. For operations such as order creation or payments, consider idempotency keys or deduplication when retries could duplicate side effects.
- Follow existing resource-disclosure conventions when choosing between `403` and `404`; do not reveal the existence of protected resources unnecessarily.

## 5. Handle Migrations Safely

- Inspect the current schema, migration history, data assumptions, deployment model, and consumers before changing persistent structures.
- A simple additive change can use a straightforward migration when its compatibility and locking impact are understood.
- For risky changes to populated data—such as adding a required field, renaming/dropping a column, or changing a type—consider staged migration, backfill, compatibility across deployed application versions, and a recovery/rollback plan.
- Never delete or discard data silently. Destructive changes require explicit authorization and a clear risk/recovery plan.
- Verify database-engine and version-specific behavior, including locking, defaults, constraint validation, and transaction support, instead of assuming every database behaves alike.

## 6. Verify Proportionately

Use the project's available checks that are relevant to the change. Do not run every tool for every task simply to complete a checklist.

- **Localized change:** run the directly relevant tests or checks and inspect the diff.
- **API or business-logic change:** test important success and failure paths, including invalid input and relevant authorization behavior.
- **Cross-component operation:** verify the contract and integration points affected by the change.
- **Migration or data-destructive change:** test against realistic existing data where feasible, check invariants, and document recovery limitations.
- **High-risk access or data operation:** verify that forbidden actions fail and legitimate operations still work.

Report which checks actually ran, their results, and what remains unverified. Never claim a migration, integration, or security behavior was tested unless it was actually checked.

## 7. Coordinate Without Over-Escalating

- `architecture`: involve when work changes service boundaries, data ownership, or significant cross-component contracts.
- `frontend`: coordinate when API behavior or contracts affect UI consumers.
- `debugging`: involve when the root cause of an existing failure is unknown.
- `security`: request deeper threat or vulnerability analysis when the risk exceeds routine implementation checks.
- `performance`: involve for measured bottlenecks or complex performance investigations.
- `testing-quality`: coordinate broader regression and integration coverage when the change or risk warrants it.

Routine CRUD, validation, and migration work stays within this skill unless a genuine trigger requires collaboration.

## Acceptance Scenarios

Use these scenarios to evaluate behavior, not as a checklist that must be executed in full for every request.

1. **CRUD endpoint:** inspect existing conventions and auth middleware; implement the required operations without inventing response formats or exposing internal fields. Verify relevant success, missing-resource, conflict, and invalid-input cases using the project's conventions.
2. **Input and access control:** reject malformed or disallowed fields, use safe queries, and verify that one user cannot read or modify another user's resource by substituting an ID. Confirm both denial and legitimate-owner access.
3. **Multi-step operation:** identify transaction boundaries and concurrency risks. If an external service is involved, do not assume a database transaction can roll it back. Simulate a relevant failure and inspect persisted state where feasible.
4. **Schema migration:** inspect existing data and deployment constraints. For a risky required-field or destructive change, plan compatibility and recovery, then verify the migration against realistic data when feasible.
5. **Retries and list endpoints:** avoid blind retries of non-idempotent operations; verify deduplication if implemented. For growing datasets, use bounded pagination consistent with the existing API and test boundary values.

For each scenario, select checks appropriate to the actual implementation and environment. These examples do not override existing contracts or justify adding infrastructure the project does not need.

## Best Practices

- Follow existing framework idioms and API conventions.
- Enforce important invariants at the database layer where appropriate, not solely through application checks.
- Prefer models and constraints that prevent invalid states when feasible.
- Keep error responses consistent; distinguish client errors from server errors according to the actual failure.
- Treat every externally reachable write path as untrusted, regardless of frontend validation.
- Keep changes focused and explain consequential trade-offs or assumptions.

## Common Pitfalls

- Trusting a request ID as proof of resource ownership.
- Passing raw request bodies to database create/update operations.
- Validating only on the frontend.
- Relying solely on application checks for uniqueness or referential integrity.
- Performing multi-step writes without required transactional or recovery handling.
- Adding required constraints to populated tables without a compatible plan.
- Retrying non-idempotent operations blindly.
- Concatenating untrusted data or identifiers into SQL.
- Returning stack traces, SQL errors, secrets, or sensitive internal objects to clients.
- Adding pagination, versioning, or elaborate migration machinery to a project when its real needs do not justify the cost.

## Completion Criteria

A backend task is complete when the requested behavior is implemented within scope, relevant contracts and data invariants are respected, appropriate checks have been performed, and remaining risks or verification gaps are disclosed. Completion means the work was verified to a level proportionate to its impact—not that every conceivable check was run.
