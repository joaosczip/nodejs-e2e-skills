---
name: testcontainers-e2e
description: Arranges JavaScript/TypeScript backend e2e test suites to run with Testcontainers, with a primary focus on provisioning isolated test databases (especially PostgreSQL) for Jest + NestJS + supertest workflows.
---

# Testcontainers E2E (JS/TS)

Use this skill when the user asks to add, update, or troubleshoot backend e2e tests that need real infrastructure (database or service containers) in JavaScript/TypeScript projects.

## Scope

- Primary target: backend endpoint e2e suites with Jest + supertest.
- Primary infrastructure pattern: dedicated PostgreSQL test database via Testcontainers.
- Secondary infrastructure pattern: additional dependencies (Redis, NATS, etc.) with containerized test services.

## Mandatory behavior for this repository

- Every backend endpoint change should include e2e coverage.
- E2e tests that depend on infrastructure must use Testcontainers, never local manually managed services.
- Keep backend endpoint tests end-to-end through NestJS DI and HTTP assertions.
- Keep multi-write backend flows transactional and synchronous.

## Workflow

1. Identify the endpoint behavior and required infrastructure.
2. Add or reuse a shared e2e container harness under `test/` (for this repo, keep backend tests in `test/*.e2e-spec.ts`).
3. Start containers in `beforeAll` (or suite-level setup) and inject dynamic connection values into app config.
4. Initialize the Nest test app via `Test.createTestingModule(...)` and `app.init()`.
5. Apply migrations/schema setup needed for the suite.
6. Execute endpoint-level HTTP assertions with `supertest`.
7. Cleanup in reverse order in `afterAll`: app first, then containers.

## Implementation rules

- Prefer typed module containers (`@testcontainers/postgresql`, etc.) over generic containers when available.
- Use dynamic mapped ports from Testcontainers; do not hardcode host ports.
- Generate unique database names per suite run when isolation is needed.
- Keep fixtures deterministic and small.
- Fail fast on startup errors; do not swallow container startup failures.
- Ensure cleanup runs even after assertion failures.

## Postgres-first setup pattern

Read `references/postgres-nestjs-jest.md` for the canonical template.

Minimal shape:

```ts
let postgres: StartedPostgreSqlContainer;

beforeAll(async () => {
  postgres = await new PostgreSqlContainer('postgres:16-alpine')
    .withDatabase('smart_budget_e2e')
    .withUsername('test')
    .withPassword('test')
    .start();

  process.env.DB_HOST = postgres.getHost();
  process.env.DB_PORT = String(postgres.getPort());
  process.env.DB_NAME = postgres.getDatabase();
  process.env.DB_USER = postgres.getUsername();
  process.env.DB_PASSWORD = postgres.getPassword();

  // build Nest app + init
});

afterAll(async () => {
  await app?.close();
  await postgres?.stop();
});
```

## Arrangement guidance for endpoint suites

- Keep one container lifecycle per e2e file unless there is a justified performance optimization.
- Use helper utilities for repetitive setup:
  - `startPostgresContainer()`
  - `buildE2eAppWithEnv()`
  - `resetDatabaseState()` when needed
- Assert both success and failure paths for each endpoint.
- Include at least one regression assertion for the bug/behavior being changed.

## Multi-service suites

When the endpoint requires more than Postgres:

- Start each dependency with Testcontainers in the same suite setup.
- Wire all connection values through env/config before app bootstrap.
- Keep service startup explicit; do not depend on host-installed daemons.

## Troubleshooting checklist

- Docker daemon unavailable: fail early with a clear error.
- Tests hanging: verify `afterAll` closes app and stops containers.
- Connection refused: verify mapped host/port values from container getters are injected before app init.
- Flaky startup: use module-specific waiting behavior and avoid fixed sleeps.

## References

- Quickstart usage summary: `references/node-testcontainers-quickstart.md`
- Canonical NestJS + Jest + supertest setup: `references/postgres-nestjs-jest.md`
